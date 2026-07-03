---
title: "📊 Pipeline Analítico com dbt"
date: 2026-06-20
tags: [dbt, SQL, DuckDB, Qualidade de Dados, Custeio]
status: "Em desenvolvimento"
links:
  code: "https://github.com/igorinacioc/orcamento-portfolio"
---

## O contexto

No hospital, os dados vêm de **3 sistemas diferentes** (CHDAs), exportados em CSV com encoding Windows-1252, separador `;`, vírgula decimal e ponto de milhar. Cada arquivo tem sua própria estrutura. Juntar isso para analisar rentabilidade por operadora era um processo manual que levava horas.

## A solução

Pipeline dbt com **29 modelos** que transforma CSVs brutos em modelos analíticos confiáveis, com **385 testes de qualidade** automatizados.

```
CSVs brutos (raw)
  │
  ├── staging (views) ── 8 modelos: atendimentos, repasse, glosas,
  │                       custo padrão, classificações, tempo cirúrgico
  │
  └── intermediate ── 21 modelos: base principal, controle de repasse,
                        glosas, WCUS, cirurgias, OPME, gratuidade
```

## Por que dbt?

| Recurso | Como uso |
|---|---|
| **Linhagem** | Sei exatamente de onde veio cada número — do CSV até o dashboard |
| **Testes** | 385 testes `not_null` + `unique` — se um CSV vem quebrado, o pipeline acusa |
| **Documentação** | Cada modelo tem schema YAML descrevendo colunas e tipos |
| **Re-executável** | `dbt run` refaz tudo do zero em ~6 segundos |

## Travas de centro de custo

O maior desafio: cada linha de custo precisa ser atribuída ao CC correto. Resolvi com **4 níveis de prioridade**:

```
1. Procedimento  → regex no nome do procedimento (ex: "tomografia" → CC 407)
2. Setor         → mapeamento pelo setor de atendimento
3. Mapeamento    → depara baseado no CC da planilha original
4. Fallback      → CC do header do atendimento (último recurso)
```

Cada linha ganha **3 flags de validação** indicando se o CC é confiável ou precisa de revisão.

## Cadeia de fallback para custo padrão

O WCUS (custo padrão) às vezes não tem todos os procedimentos. Resolvi com:

```
Porte específico → média dos portes irmãos → média histórica → 0
```

Assim nenhum atendimento fica sem custo fixo distribuído.

## 🚀 Como testar

```bash
git clone https://github.com/igorinacioc/orcamento-portfolio.git
cd orcamento-portfolio/operadoras_data

# Dados sintéticos + importação
cd ../synthetic_data && python generators/gen_operadoras.py
cp -r output/operadoras/Exemplo/* ../operadoras_data/Exemplo/
cd ../operadoras_data && python importar.py

# Rodar dbt
cd operadoras
dbt run      # 29 modelos em ~6s
dbt test     # 385 testes de qualidade
```

## Resultados

<div class="kpi-row">

<div class="kpi-card">
<div class="number">29</div>
<div class="label">modelos dbt</div>
</div>

<div class="kpi-card">
<div class="number">385</div>
<div class="label">testes</div>
</div>

<div class="kpi-card">
<div class="number">~6s</div>
<div class="label">dbt run</div>
</div>

<div class="kpi-card">
<div class="number">3</div>
<div class="label">flags de validação</div>
</div>

</div>
