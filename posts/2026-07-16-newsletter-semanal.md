---
title: "Newsletter semanal com IA: como revisitar o que voce aprendeu sem gastar tokens"
date: 2026-07-16
tags: [newsletter, IA, aprendizado, spaced-repetition]
status: rascunho
---

O problema de qualquer sistema de conhecimento pessoal e o abandono. Voce passa um fim de semana montando notas no Obsidian, fica animado, e duas semanas depois nunca mais abre. O vault vira um cemiterio de boas intencoes.

A newsletter resolve isso com um principio simples: **o sistema vai ate voce, nao o contrario**. Todo domingo as 20h, sem voce fazer nada.

## Como funciona

A newsletter e um HTML interativo servido por um container Docker local. Ela tem 6 secoes:

**O Que Voce Aprendeu Esta Semana** -- sintese narrativa conectando os temas. Se voce leu sobre EBITDA, gestao de riscos e AI na mesma semana, a newsletter mostra como essas coisas se relacionam.

**Quiz** -- 5 questoes de multipla escolha. Nao e trivia -- sao perguntas situacionais que testam compreensao real. Erros viram cartoes de revisao para a semana seguinte.

**Mapa de Calor** -- 37 conceitos em grade colorida. Verde escuro = domino (nivel 5). Cinza = ouvi falar (nivel 1). Clica na celula e ajusta seu nivel. O sistema usa isso para recomendar o que estudar.

**Desafios** -- 3 tarefas praticas por semana. Nao adianta so ler -- tem que aplicar. Exemplo real: "Identifique 1 processo de orcamento na sua empresa. Onde ele usa orcamento anual fixo e onde poderia usar rolling forecast?"

**Card do Dia** -- um conceito por dia para revisitar. Spaced repetition igual Anki, mas para ideias, nao palavras.

## O truque do buffer

Gerar uma newsletter dessas relendo 30 paginas wiki toda semana queimaria uns 50k tokens. A solucao: **buffer de fragmentos**.

Durante a ingestao de um artigo, o Claude Code ja extrai:
- highlight (o insight principal)
- conexoes cross-dominio
- 1 questao de quiz
- palavras novas em ingles
- paginas candidatas a revisao futura

Isso vira 15 linhas num arquivo. Na hora de gerar a newsletter, o sistema le o buffer (3-5k tokens) em vez de reler 30 paginas (50k tokens). Economia de 90%.

## O chat de IA

A newsletter tem um chat embutido que conversa com Ollama (modelo local, gratuito) ou Claude (nuvem, fallback). Voce le sobre convexidade, nao entendeu, pergunta ali mesmo. O chat ja carrega o contexto da newsletter + paginas wiki relevantes -- nao precisa explicar o que e duration para a IA.

Default e Ollama 14B (qwen2.5:14b). Se a resposta ficar ruim, um botao "Perguntar ao Claude" chama a API. Custo: $0 na maioria das sessoes, ~$0.03 quando usa Claude Haiku.

## Por que funciona

Tres mecanismos de retencao operando juntos:

1. **Active recall** (quiz) -- lembrar e mais eficaz que reler
2. **Spaced repetition** (card do dia, revisitando) -- intervalos crescentes
3. **Aplicacao** (desafios) -- transferencia para o mundo real

Nenhum desses mecanismos depende de disciplina -- o sistema empurra eles pra voce.
