# BLOG — Documentação da Sessão
# Data: 02/07/2026
# Objetivo: Criar blog/portfolio pessoal + preparar projetos para ser testáveis

## Arquitetura do blog

```
Blog: Hugo + Hextra + FrankMD → GitHub → GitHub Actions → Netlify
Repo: igorinacioc/igorinacioc.github.io (público)

FrankMD (editor)     Hugo (gerador)       Netlify (hospedagem)
─────────────────    ───────────────      ───────────────────
PC pessoal           PC pessoal / CI      Nuvem (gratuito)
Escreve .md     →    Gera HTML       →    Servo público
Preview live         Build estático       Deploy previews (PR)
```

## Stack

- Hugo 0.156.0 (Extended)
- Hextra theme (via Go modules)
- FrankMD como editor Markdown (instalar no PC pessoal)
- GitHub Actions para CI/CD
- Netlify para hospedagem (deploy previews em PRs)
- AWS S3 para imagens (opcional, igual Akita)

## Estrutura do repositório

```
igorinacioc.github.io/
├── .github/workflows/deploy.yaml   # CI/CD → Netlify
├── hugo.yaml                        # Config Hugo + menu
├── go.mod                           # Módulo Go (Hextra)
├── netlify.toml                     # Config Netlify
├── content/
│   ├── _index.md                    # Home (bio + skills + cards)
│   ├── projects/
│   │   ├── _index.md               # Grid de projetos
│   │   ├── sistema-orcamentario/   # Case study detalhado
│   │   ├── dados-sinteticos/       # Case study
│   │   └── pipeline-dbt/           # Case study
│   ├── blog/_index.md              # Lista de artigos
│   └── about/index.md              # Currículo expandido
└── assets/css/custom.css           # KPI cards, arch boxes, etc.
```

## Posicionamento (CRÍTICO)

Igor NÃO é dev. É profissional de Controladoria e Finanças (13 anos) que aprendeu
tecnologia para resolver problemas reais. O portfolio DEVE comunicar isso:

- Título: "Controladoria & Finanças • FP&A • Custos • Automação"
- Tom: "profissional de finanças que programa" nunca "dev"
- Projetos nasceram de necessidades reais (não são "side projects")
- Skills primárias: Excel Avançado, VBA, ERP Tasy, SQL, Python
- Formação: MBA Finanças USP ESALQ + Ciências Contábeis + CRC

## "Testar o projeto" — 3 modos

### 1. GitHub Codespaces (1 clique)
`.devcontainer/devcontainer.json` no repo orcamento-portfolio.
Recrutador clica "Code → Codespaces → Create" e em ~2min tem tudo rodando.
Setup automático: pip install + dados sintéticos + dbt run + migrate.

### 2. Docker Compose (1 comando)
`docker-compose.yml` no repo. `docker compose up` sobe Django + dados.

### 3. Local (README)
Instruções no README e na página de cada projeto.

## Netlify — por que não GitHub Pages

- Deploy previews: cada PR gera URL única para revisar
- CDN global mais rápido
- Formulários sem backend (contato)
- Akita usa Netlify → seguir referência
- Grátis (300 build minutes, 100GB bandwidth)

### Configurar Netlify (primeira vez — fazer no PC pessoal)

1. Criar conta em netlify.com (login com GitHub)
2. Importar repo igorinacioc.github.io
3. Build command: `hugo mod tidy && hugo --gc --minify`
4. Publish directory: `public`
5. Pegar NETLIFY_SITE_ID e criar NETLIFY_AUTH_TOKEN
6. Adicionar secrets no GitHub

## PRÓXIMA SESSÃO — Checklist

### No PC pessoal:

1. [ ] Clonar blog: `git clone https://github.com/igorinacioc/igorinacioc.github.io.git`
2. [ ] Instalar Hugo Extended 0.156.0
3. [ ] Rodar `hugo mod tidy && hugo server` → testar local
4. [ ] Instalar FrankMD: `curl -sL ... | bash`
5. [ ] Configurar Netlify (criar conta, pegar tokens)
6. [ ] Adicionar NETLIFY secrets no GitHub
7. [ ] Tirar screenshots dos projetos → colocar nas páginas
8. [ ] Escrever primeiro post do blog
9. [ ] Atualizar link do LinkedIn no about
10. [ ] Verificar se GitHub Pages está servindo o site (fallback)

### Pendências técnicas:

1. [ ] go.sum precisa ser gerado (`hugo mod tidy`)
2. [ ] Testar deploy no Netlify
3. [ ] Testar Codespaces no orcamento-portfolio
4. [ ] Testar docker-compose no orcamento-portfolio
5. [ ] Decidir domínio (manter igorinacioc.github.io ou comprar igorcorreia.dev?)
6. [ ] AWS S3 para imagens (seguir setup do Akita)

## Decisões tomadas

1. **Netlify > GitHub Pages** — deploy previews, formulários, CDN melhor
2. **Hextra > outros temas** — moderno, busca, dark mode, Tailwind
3. **FrankMD como editor** — igual Akita, self-hosted, sem DB
4. **3 projetos iniciais** — sistema orçamentário, dados sintéticos, pipeline dbt
5. **Página de projetos = vitrine principal** — recrutador vê em 30s
6. **"Como testar" em CADA projeto** — Codespaces + Docker + local
7. **Tom "finanças que programa"** — não "dev que fez projeto financeiro"
8. **go.mod sem go.sum** — será gerado no CI via `hugo mod tidy`
