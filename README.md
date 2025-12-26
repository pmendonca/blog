# Blog Terminal

Notas de setup rápido para rodar o site em Hugo.

## Manifesto do projeto
- `MANIFESTO.md` (diretrizes vivas: propósito, tom, e critérios de decisão)

## Pré-requisitos
- Hugo extended (use uma versão recente; o snap `extended` funciona bem).
- Git com suporte a submódulos.

## Instalação e setup
```bash
git clone https://github.com/pmendonca/blog.git
cd blog
git submodule update --init --recursive
```

## Rodando local
- Build estático: `hugo` (ou `hugo -D` para incluir drafts) → gera em `public/`.
- Servidor de preview: `hugo server -D` e acesse `http://localhost:1313`.

## Estrutura
- `content/` posts e índices (`content/posts/NN-titulo.md`).
- `themes/terminal/` tema como submódulo.
- `hugo.toml` configura site, idioma e menus.

## Dicas
- Se `hugo` não for encontrado ao instalar via snap, adicione `/snap/bin` ao `PATH`.
- Use Markdown com títulos e tom de diário técnico em português; mantenha linhas curtas.
