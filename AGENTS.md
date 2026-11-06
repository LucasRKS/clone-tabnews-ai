# curso.dev

Projeto Next.js (App Router) criado com `create-next-app`.

## Stack

- Next.js, React e TypeScript (`strict`)
- Tailwind CSS v4 via `@tailwindcss/turbopack` (não há `postcss.config` nem `tailwind.config`; o tema é configurado em CSS com `@theme inline` em `app/globals.css`)
- ESLint 9 (flat config) com `eslint-config-next` (core-web-vitals + typescript)
- Fontes Geist / Geist Mono via `next/font/google`, expostas como `--font-geist-sans` e `--font-geist-mono`

## Comandos

```bash
npm run dev      # servidor de desenvolvimento (http://localhost:3000)
npm run build    # build de produção
npm run start    # serve o build
npm run lint     # eslint
npx tsc --noEmit # checagem de tipos
```

Use `npm` como gerenciador de pacotes salvo orientação contrária.

## Estrutura

```
app/layout.tsx    # RootLayout, fontes, metadata
app/page.tsx      # Home
app/globals.css   # Tailwind + variáveis de tema (light/dark)
public/           # Arquivos estáticos
next.config.ts    # cacheComponents, partialPrefetching, regra turbopack p/ CSS
```

Alias de import: `@/*` aponta para a raiz do projeto.

## Descobertas importantes

- **Configs experimentais ligadas**: `next.config.ts` ativa `cacheComponents: true` e `partialPrefetching: true`. Com `cacheComponents`, acesso a dados dinâmicos/runtime (ex.: `cookies()`, `headers()`, `fetch` não cacheado) precisa estar dentro de `<Suspense>` ou usar `"use cache"`; do contrário o build falha.
- **Tipos globais de props**: `RootLayout` usa `LayoutProps<"/">` sem import, tipo global gerado pelo Next (arquivos em `.next/types`). Rode `npm run dev` ou `npm run build` uma vez para gerá-los antes de `tsc`. Existe também `PageProps<"/rota">` equivalente para páginas.
- **Tailwind v4**: use `@import "tailwindcss"` e tokens em `@theme`. Não crie `tailwind.config.js`. O loader está declarado em `turbopack.rules` no `next.config.ts`, então não remova essa regra.
- **Dark mode** é por `prefers-color-scheme` (variáveis `--background`/`--foreground` em `:root`), além de classes `dark:` do Tailwind.
- **Documentação da versão instalada** está em `node_modules/next/dist/docs/` (`01-app`, `02-pages`, `03-architecture`, `04-community`). Consulte essa pasta antes de assumir APIs ou convenções do Next, pois a versão pode ser mais nova que o conhecimento geral de treino.
- `.gitignore` padrão do Next; `node_modules` e `.next` não são versionados.

## Como trabalhar aqui

1. Use o App Router (`app/`). Não crie `pages/`.
2. Componentes são Server Components por padrão; adicione `"use client"` só quando precisar de estado, efeitos ou handlers de evento.
3. Estilize com classes Tailwind; valores globais de tema ficam em `app/globals.css`.
4. Use `next/image` e `next/link` em vez de `<img>` e `<a>` internos (o lint do Next cobra isso).
5. Antes de concluir uma mudança: `npm run lint` e `npm run build` (o build valida tipos e as regras de `cacheComponents`).
6. Mensagens de commit curtas e descritivas.
