# MitzIdeas — Contexto do Projeto

> Documento de referência para pessoas e agentes de IA que precisam criar, revisar ou manter o MitzIdeas. Atualizado a partir do repositório local e da configuração de deploy em 22 de setembro de 2026.

## O que é o MitzIdeas

MitzIdeas é o blog pessoal de Davi Schmitz. Ele registra carreira, estudos, hobbies, livros, experiências e ideias, com uma intenção editorial clara: consolidar aprendizado por meio da escrita sem substituir a voz autoral por texto gerado automaticamente.

O produto é um site estático, rápido e sem banco de dados ou painel administrativo. O conteúdo e a interface vivem no repositório Git; o Hugo transforma Markdown e templates em HTML, CSS e JavaScript estáticos.

**Site publicado:** <https://dsm0514.github.io/mitzideas/>  
**Repositório remoto:** <https://github.com/Dsm0514/mitzideas>

## Princípios de produto e interface

- O conteúdo é a prioridade. Evite dashboards, cards promocionais, elementos decorativos pesados e excesso de informação simultânea.
- A experiência deve ser serena, editorial, legível e minimalista. O visual é de um caderno pessoal digital, não de uma revista de marketing.
- Preserve a voz pessoal de cada artigo. IA pode ajudar em revisão e tradução, mas não deve apagar autoria, opinião ou ritmo do texto.
- Use os padrões existentes antes de propor componentes novos. Consulte `DESIGN.md` para regras acionáveis por IA e `design-system.html` para exemplos visuais executáveis.
- Não confunda o diretório `public/` com fonte: ele é uma saída gerada e está no `.gitignore`.

## Stack e arquitetura

| Camada | Implementação atual | Papel |
| --- | --- | --- |
| Gerador estático | Hugo Extended | Renderiza conteúdo, taxonomias, i18n, RSS e templates Go em site estático. |
| Versão de produção | Hugo Extended `0.146.0` | Fixada no workflow de GitHub Actions. |
| Templates | HTML + Go Templates do Hugo | `layouts/` define o shell, home, listas, páginas e parciais. |
| Estilo | CSS próprio em `assets/css/custom.css` | Tokens de tema, layout, tipografia, estados e responsividade. |
| Tipografia | DM Sans + Lora, via Google Fonts | DM Sans é a UI; Lora é a leitura editorial. |
| Conteúdo | Markdown com front matter YAML | Cada post é um page bundle com `index.md` e, quando necessário, imagens próximas ao texto. |
| JavaScript | JavaScript inline, sem framework | Busca/filtros da home e menus de tema/idioma. |
| Hospedagem | GitHub Pages | Recebe o artefato estático produzido pelo workflow. |
| Automação | GitHub Actions | Build e deploy no push para `main` ou por disparo manual. |

### Nota sobre Hextra

O histórico e alguns seletores CSS mencionam o Hextra, mas a versão atual rastreada não contém `themes/hextra`, `.gitmodules`, `go.mod` ou um campo `theme` em `hugo.toml`. Há somente entradas remanescentes de submódulo no `.git/config` local e regras CSS que ocultam elementos com nomes do Hextra. Portanto, trate os templates em `layouts/` e `assets/css/custom.css` como a fonte visual efetiva atual; não reintroduza ou atualize Hextra sem uma decisão explícita.

## Mapa do repositório

```text
.
├── .github/workflows/hugo.yaml       # build e deploy para GitHub Pages
├── archetypes/default.md             # campos iniciais de um novo Markdown
├── assets/css/custom.css             # design tokens e CSS fonte
├── content/
│   ├── pt-br/                        # idioma padrão, sem prefixo na URL
│   ├── en/                           # /en/
│   ├── es/                           # /es/
│   ├── fr/                           # /fr/
│   └── de/                           # /de/
│       ├── about/_index.md
│       └── posts/<serie>/<post>/index.md
├── i18n/<idioma>.toml                # strings da interface por idioma
├── layouts/
│   ├── baseof.html                   # shell comum: head, header, main, footer
│   ├── home.html                     # hero, busca, filtros e lista cronológica
│   ├── list.html                     # listas de taxonomia/seção
│   ├── section.html                  # página Sobre
│   ├── single.html                   # artigo e sumário lateral
│   └── _partials/                    # head, header, footer e data localizada
├── hugo.toml                         # configuração global, idiomas e taxonomias
├── DESIGN.md                         # regras de identidade para agentes
├── design-system.html                # catálogo visual interativo, fora do build
└── PROJECT_CONTEXT.md                # este documento
```

`public/`, `resources/` e `.hugo_build.lock` são gerados/localmente transitórios e não devem ser versionados.

## O que o site faz hoje

1. **Página inicial cronológica.** Agrupa artigos por mês/ano, exibe dia, título e categoria.
2. **Busca e filtros no cliente.** Busca por título e filtra por categoria, série e ano, sem servidor ou índice externo. Filtros ativos viram tags removíveis e há estado de resultado vazio.
3. **Taxonomias.** Hugo gera categorias, tags e séries; a home hoje expõe categoria e série. A política de classificação diferencia claramente assunto recorrente de sequência editorial.
4. **Artigos longos.** O template único mostra retorno à home, metadados, categorias/séries clicáveis, conteúdo Markdown e imagens responsivas.
5. **Sumário lateral.** `TableOfContents` é gerado pelo Hugo e aparece somente a partir de `1024px` de largura.
6. **Cinco idiomas.** Português brasileiro é padrão; inglês, espanhol, francês e alemão recebem suas próprias árvores de conteúdo e URLs.
7. **Troca de idioma contextual.** Quando há tradução equivalente, o menu usa `translationKey` para abrir o mesmo post em outro idioma; caso contrário, abre a home daquele idioma.
8. **Tema claro, escuro e sistema.** A escolha persiste em `localStorage` com a chave `mitz-theme`.
9. **RSS.** O Hugo expõe o feed `index.xml` para cada idioma conforme os formatos de saída disponíveis.
10. **SEO básico.** O partial `head.html` produz `title`, descrição e metatags Open Graph.

## Conteúdo existente

Há dois artigos publicados por idioma, unidos pelas mesmas `translationKey`s:

| Chave de tradução | Publicação original | Data | Série |
| --- | --- | --- | --- |
| `primeiro-post` | “Tudo tem um início” | 14/05/2026 | Tudo tem um início |
| `primeiro-post-parte-2` | “Tudo tem um início — Parte 2” | 09/06/2026 | Tudo tem um início |

Também existe uma página `about/_index.md` por idioma. O segundo post inclui imagens de tutorial ao lado do seu `index.md`, demonstrando o padrão de page bundle.

## Categorias, tags e séries

O front matter de um post contém arrays para `categories` e `series`, mas os dois campos têm funções diferentes:

| Campo | O que responde | Como usar |
| --- | --- | --- |
| `categories` | “Sobre qual assunto recorrente este post é?” | Aplique uma categoria principal e, no máximo, outras duas quando também forem substanciais. Categorias são estantes reutilizáveis para descoberta: por exemplo, `Front-End`, `Back-End`, `IA` e `Redes`. |
| `series` | “A qual jornada, projeto, livro ou sequência este post pertence?” | Use somente quando há uma sequência editorial nomeada. Um post pode ter categorias e não fazer parte de série. Normalmente, use uma série. |
| `tags` | “Qual etiqueta livre e complementar se aplica?” | A taxonomia existe no Hugo, mas não possui convenção definida nem filtro na home. Não use como sinônimo de categoria até que uma política de tags seja criada. |

**Regra para `Início`.** A categoria `Início` é histórica e limitada aos primeiros posts que contam como o MitzIdeas foi criado. Ela não deve virar uma categoria genérica para posts introdutórios, para a primeira parte de qualquer série ou para conteúdo de iniciantes.

**Regra para `Tudo tem um início`.** Esta é a série específica sobre a construção do blog com Hugo. Um artigo que apenas cite Hugo ou um recomeço pessoal não entra nela automaticamente.

**Exemplo futuro.** Uma sequência de resumos e aprendizados do livro pode usar `series: ["Entendendo Algoritmos — Aditya Y. Bhargava"]`. A categoria deve descrever o assunto reutilizável do post — por exemplo, `Algoritmos` quando essa categoria for criada — e não repetir o nome da série. Outros posts sobre algoritmos podem usar a categoria sem pertencer à série do livro.

Antes de classificar um post, agentes devem carregar `.claude/skills/mitzideas-taxonomy/SKILL.md`. A skill impede rótulos redundantes, preserva o escopo de `Início`, orienta a criação de categorias duráveis e mantém os nomes localizados de forma consistente.

## Como o conteúdo é criado

### Formato de um post

Todo artigo usa Markdown com front matter YAML. A estrutura recomendada é uma pasta por post em cada idioma:

```text
content/pt-br/posts/<slug-da-serie>/<slug-do-post>/
├── index.md
└── imagem-ou-subpasta-de-imagens.ext
```

O `index.md` começa assim:

```yaml
---
title: "Título no idioma atual"
slug: "url-localizada-do-post"
date: 2026-09-22T10:00:00-03:00
draft: true
categories: ["Categoria localizada"]
series: ["Série localizada"]
description: "Resumo curto usado como descrição da página."
translationKey: "chave-estável-compartilhada-entre-idiomas"
---
```

Depois do front matter, escreva Markdown normal. O projeto permite HTML bruto porque `markup.goldmark.renderer.unsafe = true`; use esse recurso apenas quando Markdown não bastar, pois HTML inserido no conteúdo passa a ser código confiável no site gerado.

Recursos já usados pelos posts:

- títulos `##` e `###` (também alimentam o sumário do artigo);
- parágrafos, listas, ênfase, citações e linhas horizontais;
- código inline e blocos cercados por crases;
- shortcodes de figura, por exemplo `{{< figure src="imagem.png" title="..." alt="..." >}}`;
- imagens locais posicionadas dentro do bundle do post.

### Criando um rascunho

Na raiz do repositório, o caminho mais seguro é criar primeiro o bundle PT-BR:

```bash
hugo new content/pt-br/posts/<serie>/<post>/index.md
```

O comando usa `archetypes/default.md` e cria os campos padrão. Preencha o front matter, mantenha `draft: true` enquanto o texto não estiver pronto e mude para `false` para publicar no build normal.

### Traduções: regra obrigatória

O projeto usa **diretórios de conteúdo separados por idioma**, não sufixos como `post.en.md`. Para um post publicado, crie também:

```text
content/en/posts/<serie>/<post>/index.md
content/es/posts/<serie>/<post>/index.md
content/fr/posts/<serie>/<post>/index.md
content/de/posts/<serie>/<post>/index.md
```

Em cada versão:

- traduza título, `slug`, `description`, texto, categorias e série;
- preserve exatamente a mesma data e a mesma `translationKey`;
- mantenha os assets que o Markdown daquela versão referencia;
- confira no seletor de idioma se cada página leva para sua equivalente.

Strings de interface — como “Buscar posts”, “Tema” e estados vazios — não ficam no Markdown. Elas vivem nos arquivos `i18n/pt-br.toml`, `i18n/en.toml`, `i18n/es.toml`, `i18n/fr.toml` e `i18n/de.toml`, chamados nos templates por `{{ T "chave" }}`.

## Desenvolvimento local

### Pré-requisitos

- Git;
- Hugo **Extended**. Para máxima fidelidade ao deploy, prefira `0.146.0`; o ambiente local analisado tinha `0.147.1 extended`.
- Acesso à internet ao abrir o site, para carregar DM Sans e Lora do Google Fonts.

### Iniciar o servidor

Na raiz do projeto:

```bash
hugo server
```

O Hugo inicia o servidor local, observa alterações e atualiza a página. Abra a URL indicada pelo terminal — normalmente `http://localhost:1313/mitzideas/` neste projeto. Para visualizar também rascunhos, execute:

```bash
hugo server -D
```

Antes de encerrar uma mudança visual ou editorial, valide pelo menos a home, um artigo, a página Sobre, os cinco idiomas, a troca de tema, busca/filtros e a versão desktop do sumário.

### Build de produção local

```bash
hugo --gc --minify
```

Esse comando gera a saída em `public/`. Ela é ignorada pelo Git e é reconstituída pelo CI, portanto não a adicione ao commit.

## Deploy automático: atenção antes de fazer push

O arquivo `.github/workflows/hugo.yaml` confirma a suspeita: **todo push para `main` dispara um deploy automático**. Ele também pode ser iniciado manualmente pela interface do GitHub (`workflow_dispatch`).

Fluxo executado pelo GitHub Actions:

1. Roda no `ubuntu-latest`.
2. Instala Hugo Extended `0.146.0` e Dart Sass.
3. Faz checkout do repositório com submódulos recursivos e histórico completo.
4. Configura GitHub Pages.
5. Executa `hugo --gc --minify --baseURL "${{ steps.pages.outputs.base_url }}/"`.
6. Publica `./public` como artefato Pages.
7. Após o build, o job de deploy entrega esse artefato no ambiente `github-pages`.

O workflow possui grupo de concorrência `pages` e **não cancela** deploys que já estão em andamento. Isso significa que pushes consecutivos entram em fila em vez de substituir o deploy anterior.

### Checklist antes do push

- [ ] Rode `hugo server` e teste a mudança localmente.
- [ ] Rode `hugo --gc --minify` sem erro.
- [ ] Para conteúdo novo, valide as cinco traduções e suas `translationKey`s.
- [ ] Não adicione `public/`, `resources/` ou `.hugo_build.lock`.
- [ ] Revise o diff com cuidado: o push em `main` publica a versão resultante.
- [ ] Depois do push, acompanhe a execução “Deploy Hugo site to Pages” na aba Actions do GitHub.

## Guia de manutenção por área

| Objetivo | Arquivo(s) a alterar |
| --- | --- |
| Cor, tipografia, espaçamento, estados ou responsividade | `assets/css/custom.css` e a referência em `DESIGN.md`/`design-system.html` |
| Cabeçalho, menu de tema ou idioma | `layouts/_partials/header.html` |
| Metadados, fontes ou CSS processado pelo Hugo Pipes | `layouts/_partials/head.html` |
| Página inicial, busca e filtros | `layouts/home.html` |
| Página de artigo e sumário | `layouts/single.html` |
| Listas de taxonomia | `layouts/list.html` |
| Página Sobre | `layouts/section.html` e `content/<idioma>/about/_index.md` |
| Datas por idioma | `layouts/_partials/date-i18n.html` |
| Texto de interface | `i18n/<idioma>.toml` |
| Idiomas, URLs, taxonomias e parâmetros globais | `hugo.toml` |
| Processo de publicação | `.github/workflows/hugo.yaml` |

## Regras para agentes de IA

1. Leia este arquivo e `DESIGN.md` antes de implementar uma tela ou alterar conteúdo estrutural.
2. Use `design-system.html` como referência visual; ele demonstra apenas padrões que existem hoje.
3. Antes de criar ou revisar uma tradução em `content/`, carregue `.claude/skills/mitzideas-translation/SKILL.md`. Ela protege front matter, `translationKey`, sintaxe Hugo, código, links, assets e a voz do autor.
4. Antes de definir ou alterar `categories`/`series`, carregue `.claude/skills/mitzideas-taxonomy/SKILL.md`.
5. Use `.claude/skills/humanizer/SKILL.md` apenas se for solicitado humanizar texto gerado ou traduzido; não aplique automaticamente a texto já autoral.
6. Preserve o sistema multilíngue completo. Não crie uma página somente em PT-BR sem explicitar que a tradução está pendente.
7. Reutilize classes e tokens de `assets/css/custom.css`; não introduza bibliotecas de UI, frameworks JavaScript ou novas fontes sem autorização.
8. Não edite `public/`: altere fontes (`content/`, `layouts/`, `assets/`, `i18n/`), depois gere o site.
9. Não altere `.github/workflows/hugo.yaml`, `main` ou configurações de Pages sem tratar isso como mudança de produção.
10. Ao mudar um padrão visual existente, atualize `DESIGN.md` e `design-system.html` no mesmo commit.
