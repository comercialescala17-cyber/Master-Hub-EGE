# CODEX PROMPT - EGE Platform

Use este arquivo como contexto inicial para Codex, Cursor, Copilot ou outro assistente de IA ao trabalhar neste workspace.

## Objetivo

Este workspace contem o EGE Platform Hub, um hub de documentacao estrategica para o projeto Enterprise Growth Ecosystem.

O site principal ja foi publicado no GitHub Pages:

- Site: https://comercialescala17-cyber.github.io/Master-Hub-EGE/
- Repo: https://github.com/comercialescala17-cyber/Master-Hub-EGE
- Fonte local principal: `ege-framework/`
- Build publicado localmente em: `github-master-hub/`

## Estrutura Local

```text
portfolio-nextjs-v2-final/
  ege-framework/       Projeto Next.js principal do EGE Hub
  github-master-hub/   Clone do repo GitHub Pages publicado
  portfolio/           Portfolio Next.js separado
  n8n/                 Workflows de automacao
  tools/               Node.js portatil usado neste Windows
  tiiny-site.html      HTML original importado do Tiiny
```

## Stack

EGE Framework:

```text
Next.js 15.2.0
React 19.0.0
TypeScript 5.7
CSS puro
Sem Tailwind
Sem shadcn
Sem Radix
```

Observacao importante: `next@15.2.0` possui aviso de vulnerabilidade critica no `npm install`. O projeto funciona, mas deve ser atualizado em uma proxima rodada.

## Node e npm

Nesta maquina, `node` e `npm` nao estavam disponiveis no PATH global. Foi instalado um Node portatil em:

```text
tools/node-v24.15.0-win-x64/
```

Para rodar comandos no PowerShell:

```powershell
$nodeDir = Resolve-Path "..\tools\node-v24.15.0-win-x64"
& "$nodeDir\npm.cmd" install
```

Para evitar erro de `npm.ps1` bloqueado por Execution Policy, use sempre `npm.cmd`.

## Comandos Principais

Instalar dependencias:

```powershell
cd ege-framework
$nodeDir = Resolve-Path "..\tools\node-v24.15.0-win-x64"
& "$nodeDir\npm.cmd" install
```

Rodar dev server:

```powershell
cd ege-framework
$nodeDir = Resolve-Path "..\tools\node-v24.15.0-win-x64"
& "$nodeDir\node.exe" node_modules\next\dist\bin\next dev -p 3002 -H 127.0.0.1
```

URL local usada:

```text
http://127.0.0.1:3002
```

Gerar build estatico para GitHub Pages:

```powershell
cd ege-framework
$nodeDir = Resolve-Path "..\tools\node-v24.15.0-win-x64"
$npm = Join-Path $nodeDir "npm.cmd"
cmd.exe /c "set PATH=$nodeDir;%PATH%&& `"$npm`" run build"
```

## Deploy no GitHub Pages

O `ege-framework/next.config.mjs` esta configurado para export estatico em GitHub Pages:

```js
const nextConfig = {
  output: 'export',
  trailingSlash: true,
  basePath: '/Master-Hub-EGE',
  assetPrefix: '/Master-Hub-EGE/',
};
```

Apos o build, copie `ege-framework/out/*` para `github-master-hub/`, mantendo a pasta `.git`, crie `.nojekyll`, commit e push:

```powershell
cd ege-framework
$repo = Resolve-Path "..\github-master-hub"
$out = Resolve-Path ".\out"

Get-ChildItem -LiteralPath $repo.Path -Force |
  Where-Object { $_.Name -ne ".git" } |
  Remove-Item -Recurse -Force

Copy-Item -Path (Join-Path $out.Path "*") -Destination $repo.Path -Recurse -Force
New-Item -ItemType File -Path (Join-Path $repo.Path ".nojekyll") -Force | Out-Null

cd ..\github-master-hub
git add -A
git commit -m "Deploy EGE framework Next export"
git push origin main
```

## Arquitetura do EGE Framework

```text
ege-framework/
  app/
    layout.tsx
    page.tsx
    globals.css
    docs/[id]/page.tsx
    flows/page.tsx
  components/
    Topbar.tsx
    Sidebar.tsx
    DocCard.tsx
    DocViewer.tsx
    KpiRow.tsx
    FlowCanvas.tsx
    CopyButton.tsx
  lib/
    docs.ts
  public/
    docs/
    n8n/
  scripts/
    extract-docs.mjs
```

## Regras de Codigo

1. Use TypeScript estrito.
2. Use Server Components por padrao.
3. Use `'use client'` apenas quando houver hooks, eventos ou APIs do navegador.
4. Em rotas dinamicas do Next 15, trate `params` como async quando necessario.
5. Use imports com `@/`, por exemplo `@/components/Topbar`.
6. Nao use Tailwind.
7. Nao adicione bibliotecas de UI sem necessidade.
8. Preserve o estilo visual atual: dark UI, sidebar fixa, topbar, cards densos.
9. Para docs, a fonte de verdade e `lib/docs.ts`.
10. Para arquivos HTML dos documentos, use `public/docs/[id].html`.

## Fluxo Recomendado Para Mudancas

1. Alterar codigo em `ege-framework/`.
2. Rodar build.
3. Testar localmente em `http://127.0.0.1:3002`.
4. Exportar para `out/`.
5. Copiar `out/` para `github-master-hub/`.
6. Commit e push para `main`.
7. Verificar:

```text
https://comercialescala17-cyber.github.io/Master-Hub-EGE/
```

## Estado Atual

- Dependencias instaladas em `ege-framework/node_modules`.
- Dev server rodou com sucesso em `127.0.0.1:3002`.
- GitHub Pages ativado e respondendo `200 OK`.
- Ultimo deploy conhecido: `3df6f2d Deploy EGE framework Next export`.

