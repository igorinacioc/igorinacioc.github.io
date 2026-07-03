---
title: "🏥 Sistema Orçamentário Hospitalar"
date: 2026-06-01
tags: [Django, DuckDB, dbt, Chart.js, Python, SQL, Controladoria]
status: "Em desenvolvimento"
links:
  code: "https://github.com/igorinacioc/orcamento-portfolio"
---

## A origem

Trabalho há anos com controle orçamentário hospitalar usando Excel, ERP Tasy e BI WeKnow. O processo era manual: importar planilhas, consolidar centros de custo, conferir desvios, gerar apresentação. **71 centros de custo, R$ 140 milhões em despesas anuais.** Planilha não escala.

Aprendi Python e SQL para resolver meu próprio problema. O resultado: um sistema que automatiza o que eu fazia manualmente.

## O que o sistema faz

```
┌──────────────────────────────────────────────────────┐
│  MÓDULO ORÇAMENTO (SQLite)                            │
│  ┌──────────┐  ┌──────────┐  ┌──────────────────┐   │
│  │ Import   │  │ Liberação│  │ Tratativas        │   │
│  │ Excel    │→ │ CC × Mês │→ │ (comentários,      │   │
│  │ (upsert) │  │          │  │  anexos, SLA)      │   │
│  └──────────┘  └──────────┘  └──────────────────┘   │
├──────────────────────────────────────────────────────┤
│  MÓDULO OPERADORAS (DuckDB + dbt)                     │
│  ┌──────────┐  ┌──────────┐  ┌──────────────────┐   │
│  │ CSVs     │  │ dbt (29  │  │ Dashboards        │   │
│  │ (3 CHDAs)│→ │ modelos) │→ │ (rentabilidade,   │   │
│  │          │  │          │  │  cirurgia, OPME)   │   │
│  └──────────┘  └──────────┘  └──────────────────┘   │
└──────────────────────────────────────────────────────┘
```

## Stack — por que essas escolhas

| Tecnologia | Por que usei |
|---|---|
| **Django** | Framework web completo em Python — admin, auth, ORM |
| **DuckDB** | Banco OLAP embarcado, zero infra — análise pesada local |
| **dbt** | Transformação de dados com testes — 385 testes de qualidade |
| **Chart.js** | Gráficos interativos sem dependência paga |
| **HTMX** | Atualizações dinâmicas sem JavaScript complexo |
| **SQLite** | Banco simples para dados transacionais (orçamento) |

> **Nota:** aprendi essas tecnologias do zero, fora do horário de trabalho. Tudo está documentado no GitHub e neste blog.

## Funcionalidades que substituem planilhas

- **Importação Excel automatizada** — upsert inteligente que reconhece centro de custo, conta contábil e competência
- **Dashboards por operadora** — rentabilidade por convênio que o ERP não entrega
- **Tratativas integradas** — comentários, anexos e SLA para desvios orçamentários (antes: e-mail e WhatsApp)
- **Pipeline dbt** — transformação de CSVs hospitalares em modelos analíticos, com rastreabilidade
- **Alertas automáticos** — e-mail quando um CC estoura o orçamento

## Números (com dados sintéticos)

<div class="kpi-row">

<div class="kpi-card">
<div class="number">45K</div>
<div class="label">lançamentos</div>
</div>

<div class="kpi-card">
<div class="number">29</div>
<div class="label">modelos dbt</div>
</div>

<div class="kpi-card">
<div class="number">385</div>
<div class="label">testes de qualidade</div>
</div>

<div class="kpi-card">
<div class="number">7</div>
<div class="label">dashboards</div>
</div>

</div>

## 🚀 Como testar

Quer ver o sistema funcionando? São 2 comandos:

```bash
git clone https://github.com/igorinacioc/orcamento-portfolio.git
cd orcamento-portfolio

# Gerar dados + importar + dbt + servidor
python synthetic_data/generators/gen_operadoras.py
cp -r synthetic_data/output/operadoras/Exemplo/* operadoras_data/Exemplo/
cd operadoras_data && python importar.py && cd operadoras && dbt run && cd ../..
python manage.py runserver 0.0.0.0:8001
```

Abra `http://localhost:8001` e faça login:

| Usuário | Senha | Perfil |
|---|---|---|
| `admin` | `admin` | Controladoria (acesso total) |
| `gestor` | `gestor123` | Visão do gestor de CC |
| `diretoria` | `dir123` | Visão consolidada |

**Requisitos:** Python 3.10+, DuckDB, dbt-duckdb.

> Em breve: GitHub Codespaces (1 clique, zero instalação) e Docker Compose.
