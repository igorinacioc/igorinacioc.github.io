---
title: "🔬 Gerador de Dados Sintéticos Hospitalares"
date: 2026-06-15
tags: [Python, DuckDB, SQL, Dados Sintéticos]
status: "Concluído"
links:
  code: "https://github.com/igorinacioc/orcamento-portfolio/tree/master/synthetic_data"
---

## O problema

Trabalho com dados reais de hospital — **LGPD proíbe expor**. Para mostrar o sistema no portfólio, eu precisava de um dataset realista mas 100% anonimizado que simulasse o comportamento de um hospital de verdade.

Comprar dados sintéticos? Não existe pronto. Gerar manualmente? 40 arquivos CSV com milhares de linhas. Impossível.

## A solução

Gerador em Python que usa **DuckDB como engine SQL vetorizada** — zero loops Python para gerar dados. Tudo em SQL, gerando 40 CSVs em segundos.

### Por que DuckDB e não só Pandas?

```python
# Loop Python (lento) — NUNCA usei isso
for i in range(45000):
    df.loc[i] = [gerar_cc(), gerar_data(), gerar_valor()]

# SQL vetorizado (rápido) — foi ASSIM que fiz
conn.execute("""
    INSERT INTO lancamentos
    SELECT
        'CC-' || FLOOR(RANDOM() * 71 + 1) AS centro_custo,
        DATE '2026-01-01' + FLOOR(RANDOM() * 30) AS data,
        ROUND(EXP(RANDOM() * 10 + 5), 2) AS valor
    FROM GENERATE_SERIES(1, 45000);
""")
```

A diferença: **segundos vs minutos**. DuckDB processa tudo em memória, vetorizado, sem sair do Python.

## Técnicas de modelagem

| Aspecto | Técnica |
|---|---|
| **Valores financeiros** | Distribuição log-normal (`EXP(RANDOM())`) — poucos valores altos, muitos baixos (igual hospital real) |
| **Sazonalidade** | Mais atendimentos em dias úteis (padrão hospitalar real) |
| **Mix de convênios** | Proporções baseadas em dados reais do setor (anonimizadas) |
| **Estrutura de custos** | Custeio por absorção — custo fixo rateado por procedimento |

## Resultados

<div class="kpi-row">

<div class="kpi-card">
<div class="number">40</div>
<div class="label">CSVs gerados</div>
</div>

<div class="kpi-card">
<div class="number">&lt;5s</div>
<div class="label">tempo de geração</div>
</div>

<div class="kpi-card">
<div class="number">~800</div>
<div class="label">atendimentos/mês</div>
</div>

<div class="kpi-card">
<div class="number">~3.200</div>
<div class="label">linhas base</div>
</div>

</div>

## 🚀 Como testar

```bash
git clone https://github.com/igorinacioc/orcamento-portfolio.git
cd orcamento-portfolio/synthetic_data
python generators/gen_operadoras.py
# 40 CSVs gerados em output/operadoras/Exemplo/
```
