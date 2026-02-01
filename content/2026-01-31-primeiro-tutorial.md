---
title: "Como criar um blog com Marmite"
slug: como-criar-blog-marmite
date: 2026-01-31
tags: tutorial, marmite, rust, blog
author: fabricio
---

# Como criar um blog com Marmite

Neste tutorial, vou mostrar como criar seu próprio blog usando o **Marmite**, um gerador de sites estáticos escrito em Rust.

## O que é o Marmite?

[Marmite](https://marmite.blog/) é um gerador de sites estáticos minimalista que transforma arquivos Markdown em um blog completo. Suas principais características são:

- **Zero configuração** para começar
- **Escrito em Rust** — rápido e eficiente
- **Temas e colorschemes** incluídos
- **Busca integrada**
- **RSS Feed**
- **Suporte a tags e arquivos**

## Instalação

A forma mais fácil de instalar é usando o script de instalação:

```bash
curl -sS https://marmite.blog/install.sh | sh
```

Ou se você tem Rust instalado:

```bash
cargo install marmite
```

## Criando seu primeiro blog

1. **Crie uma pasta para o conteúdo:**

```bash
mkdir meu-blog
cd meu-blog
```

2. **Inicialize o blog:**

```bash
marmite . --init-site \
    --name "Meu Blog" \
    --tagline "Minha descrição" \
    --enable-search true
```

3. **Crie seu primeiro post:**

```bash
marmite . --new "Meu Primeiro Post" -t "novo,post"
```

4. **Gere e visualize o site:**

```bash
marmite . --serve
```

Acesse `http://localhost:8000` e veja seu blog funcionando!

## Estrutura de um post

Os posts são arquivos Markdown com um frontmatter YAML:

```markdown
---
title: "Título do Post"
date: 2026-01-31
tags: tag1, tag2
---

Conteúdo do post aqui...
```

## Próximos passos

- Personalize o `marmite.yaml` com suas informações
- Explore os colorschemes disponíveis
- Adicione mais conteúdo!

---

Gostou? Deixe seu comentário!
