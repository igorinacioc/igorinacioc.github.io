---
title: "Como montei um Segundo Cerebro com IA para aprender financas"
date: 2026-07-16
tags: [second-brain, aprendizado, IA, financas]
status: rascunho
---

Eu lia muito e esquecia quase tudo. Artigos, livros, aulas, newsletters -- entrava por um olho e saia pelo outro. Pior: quando precisava aplicar um conceito no trabalho, lembrava que ja tinha lido sobre aquilo mas nao sabia onde.

O Segundo Cerebro resolveu isso. Em 4 dias, processei 50 artigos e montei uma wiki pessoal com 37 conceitos interconectados. Agora toda semana recebo uma newsletter com quiz, desafios e recomendacoes do que estudar. E tudo que produzo vai para este blog.

## O problema: consumo sem retencao

Trabalho com controladoria e financas ha 13 anos. Leio bastante -- artigos tecnicos, livros, aulas de MBA. Mas o padrao era sempre o mesmo:

- Li um artigo excelente sobre EBITDA e armadilha de valor
- Uma semana depois, lembro que li mas nao lembro os detalhes
- No trabalho, continuo usando as mesmas ferramentas de sempre
- O conhecimento novo nao vira pratica

A conta nao fecha: horas lendo, zero de output. Todo mundo que estuda serio passa por isso.

## A solucao: LLM Wiki + newsletter + blog

O sistema tem tres camadas:

**1. Wiki pessoal (Obsidian + Claude Code)**

Inspirado no padrao LLM Wiki do Andrej Karpathy. Cada artigo que leio vira paginas atomicas na wiki -- uma ideia por pagina, conectadas entre si. O Claude Code processa as fontes e cria as paginas, mas eu reviso e ajusto.

Em 4 dias: 50 paginas, 4 dominios (financas, banking, tecnologia, carreira), 16 hubs de conexao.

**2. Newsletter semanal interativa**

Todo domingo, o sistema gera uma newsletter com:
- Sintese do que aprendi na semana
- Quiz de 5 questoes (active recall)
- Desafios praticos para aplicar os conceitos
- Mapa de proficiencia: sei nivel 4 em EBITDA, nivel 1 em convexidade
- Recomendacoes do que estudar na semana seguinte

A newsletter roda num servidor Docker local com chat de IA integrado. Posso tirar duvidas na hora -- a IA explica o conceito com contexto da minha propria wiki.

**3. Blog (este site)**

O output final. Textos que escrevo viram posts aqui. Projetos que construo viram paginas com explicacao detalhada. O blog fecha o ciclo: consumir -> reter -> aplicar -> publicar.

## A stack tecnica

| Componente | Ferramenta |
|---|---|
| Wiki | Obsidian + Claude Code (DeepSeek V3) |
| Newsletter | Node.js + SQLite + Docker |
| Chat IA | Ollama (qwen2.5:14b local, gratuito) |
| Blog | GitHub Pages + HTML/CSS vanilla |

Tudo roda local. O unico custo e a geracao da newsletter (~$0.005/semana com DeepSeek). O chat usa modelo local gratuito.

## O que aprendi montando isso

Montar o sistema foi tao valioso quanto usa-lo. Algumas licoes:

- **Buffer de fragmentos**: em vez de reprocessar 30 paginas a cada newsletter, cada ingestao ja extrai highlights, quiz e conexoes. A newsletter so monta. Economia de 90% em tokens.

- **Heatmap de proficiencia**: so de ver que eu era nivel 1 em 5 conceitos de banking, percebi o vies do que eu leio vs o que eu preciso aprender.

- **Active recall > releitura**: reler a pagina wiki nao fixa. Responder um quiz sobre o conceito 3 semanas depois, sim.

- **Output e o gargalo real**: o sistema e bom em input e retencao. Mas subir de nivel requer aplicar e escrever. Este blog e a peca que faltava.

## Proximos passos

O sistema tem 37 conceitos trackeados. A meta e chegar a 100 em 3 meses, com pelo menos 1 post publicado por semana. Os projetos do portfolio (sistema orcamentario, pipeline dbt, dados sinteticos) vao ganhar atualizacoes conforme aplico os conceitos novos.

Se voce tambem esquece o que le, experimente montar algo parecido. Nao precisa de 50 paginas no primeiro dia -- comece com 3 artigos e uma planilha de conceitos.
