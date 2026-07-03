# igorinacioc.github.io

Blog e portfolio — Hugo + Hextra + FrankMD, deploy no Netlify.

## Setup local

```bash
# Pré-requisitos: Hugo Extended >= 0.156.0, Go >= 1.24

git clone https://github.com/igorinacioc/igorinacioc.github.io.git
cd igorinacioc.github.io
hugo mod tidy
hugo server --disableFastRender
# → http://localhost:1313
```

## FrankMD (editor Markdown)

```bash
# Instalar (PC pessoal)
curl -sL https://raw.githubusercontent.com/akitaonrails/FrankMD/master/install.sh | bash
source ~/.config/frankmd/fed.sh

# Abrir na pasta do blog
fed ~/igorinacioc.github.io
```

## Estrutura

```
content/
├── _index.md              ← Home
├── projects/              ← Portfolio
│   ├── _index.md
│   ├── sistema-orcamentario/
│   ├── dados-sinteticos/
│   └── pipeline-dbt/
├── blog/                  ← Artigos
└── about/                 ← Currículo e trajetória
```

## Deploy

Push na `main` → GitHub Actions builda Hugo → deploy no Netlify.
PRs geram deploy preview automático (URL temporária pra revisar antes de publicar).

## Configurar Netlify (primeira vez)

1. Criar conta em [netlify.com](https://netlify.com) (login com GitHub)
2. Criar novo site "Import from GitHub" → escolher este repo
3. Build command: `hugo mod tidy && hugo --gc --minify`
4. Publish directory: `public`
5. Pegar `NETLIFY_SITE_ID` e criar `NETLIFY_AUTH_TOKEN` em User Settings → Applications
6. Adicionar os dois como secrets no GitHub: Settings → Secrets → Actions
