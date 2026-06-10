---
title: "Tudo tem um início — Parte 2"
slug: "tudo-tem-um-inicio-parte-2"
date: 2026-06-09T10:12:00-03:00
draft: false
categories: ["Início"]
series: ["Tudo tem um início"]
description: "A segunda parte da construção do MitzIdeas, focando na instalação e configuração do Hugo."
translationKey: "primeiro-post-parte-2"
---

## Introdução

No post anterior, compartilhei a ideia por trás do nascimento deste blog. Hoje, vamos abrir o capô da máquina e falar sobre a engenharia que faz o MitzIdeas funcionar no ar sem me custar um único centavo de hospedagem, mantendo uma velocidade absurda e controle total sobre cada pixel.

Tentarei falar um pouco mais sobre toda a estrutura que mantém este blog de pé, explicar melhor cada conceito, e no fim oferecer um mini tutorial ajudando você a partir do zero para seu primeiro blog feito com Hugo.

---

## O Motor: Entendendo o Hugo

O Hugo é um **Gerador de Sites Estáticos (Static Sites Generator, SSG)** escrito na linguagem de programação **Go**. Ele foi criado por **Steve Francia** (conhecido como *spf13* no ecossistema Golang) em 2013, que desenhou a estrutura base de como o projeto processa os templates e a famosa lógica de *lookup order*. 

Por volta de 2015, a liderança foi passada para **Bjorn Erik Pedersen** (*bep*), que transformou esse motor em um verdadeiro carro de corrida profissional. Entre os feitos mais notáveis do Bjorn, destacam-se:

* **Performance Extrema:** Otimização do compilador para tirar o máximo proveito de processadores multi-core, fazendo com que sites com dezenas de milhares de páginas sejam compilados em poucos segundos.
* **Hugo Pipes:** Processamento nativo de assets, minificação, fingerprinting e suporte robusto a multilinguismo (`i18n`).
* **Comunidade:** Uma dedicação quase sobre-humana em responder *issues* e revisar *pull requests* no GitHub, mantendo o projeto na vanguarda da Web.

Por causa desse framework flexível, o Hugo é amplamente utilizado para criar portais de documentação, sites governamentais, corporativos e, pasmem, blogs pessoais. Ele atua como um ótimo **Content Management System (CMS)** de arquivos, onde o seu editor de código (como o VS Code) vira o seu painel de controle.

---

## Por que usar Hugo em vez de HTML, CSS e JavaScript puros?

Se você criar um site apenas com HTML, CSS e JS "puro" (o famoso *Vanilla*), você está construindo uma casa tijolo por tijolo. Funciona maravilhosamente bem para 3 ou 5 páginas. Mas e quando você tiver 100 posts?

Imagine que eu tenha 50 posts e decida mudar o link da página "Sobre" no cabeçalho:
* **No HTML puro:** Eu teria que abrir **51 arquivos** (os 50 posts + a página `index.html`) e alterar manualmente um por um. Se esquecer de um, gerei uma inconsistência. 
* **No Hugo:** Eu altero um único arquivo parcial (`layouts/_partials/header.html`) e, ao rodar a compilação, o Hugo injeta essa mudança em todas as páginas instantaneamente. O Hugo é uma **fábrica de páginas**.

Além disso, escrever em Markdown (`.md`) é infinitamente mais produtivo do que ficar montando tags HTML igual a peças de LEGO. Você foca no texto, e o motor cuida do resto. 

O ecossistema ainda traz vantagens como **Minificação** (remoção de espaços em branco do código) e **Fingerprinting** (técnica de *Cache Busting* que adiciona um *hash* único ao nome do arquivo, como `custom.min.8723.css`, forçando o navegador do usuário a baixar a versão mais nova e ignorar o cache antigo).

### Quando NÃO usar Hugo?
Não estou aqui para vender o Hugo como bala de prata. Ele é excessivo para *Landing Pages* simples de uma página só. Também não é ideal para sistemas que precisam de dados processados em tempo real no servidor, como painéis com login de usuários ou feeds dinâmicos de redes sociais. Nesses cenários, frameworks como o Django (que uso no meu dia a dia) continuam sendo a escolha correta.

---

## Recursos Fundamentais de Arquitetura

### 1. Servidor de Desenvolvimento Integrado
Quando você digita `hugo server` no terminal, ele levanta um servidor local que cria um "túnel" entre seus arquivos e o navegador. Toda vez que você salva um arquivo, ele detecta as mudanças e recarrega a página em milissegundos. 

Como o Hugo é feito em Go, temos acesso aos **Go Templates** para interpolar e transformar dados dinâmicos. Quem vem do ecossistema Python/Django vai notar uma similaridade imediata. É um ambiente totalmente *batteries included*.

### 2. Hierarquia de Layouts
O Hugo trabalha com fôrmas. Você define um arquivo principal chamado `baseof.html` com o esqueleto do site (o que se repete em tudo, como header e footer) e abre um "buraco" usando o comando `{{ block "main" . }}{{ end }}`. Os outros templates apenas herdam essa fôrma e preenchem o buraco.

### 3. Taxonomias Nativas
Classificar conteúdo no Hugo é extremamente poderoso e não exige plugins. O sistema de **taxonomias** cria relações lógicas complexas (como tags, categorias e séries) gerando um índice invertido e ponderado. Ele sabe exatamente quais posts são relacionados entre si sem precisar fazer uma única query SQL.

### 4. Dicionário de Arquivos Estranhos
* `archetypes/`: Guarda seus templates de rascunho. O arquivo `default.md` define quais campos virão preenchidos por padrão toda vez que você criar um post novo.
* `.toml`: O formato de arquivo de configuração (como o `hugo.toml`). É o cérebro global do site.
* `single.html` vs `list.html`: O primeiro dita o visual de um post único; o segundo renderiza páginas que listam múltiplos posts (como os índices de categorias).

---

## O Modelo de Segurança: "Confiança Restrita"

O Hugo opera sob uma premissa muito clara: **ele confia em quem escreve o código (o desenvolvedor), mas não confia em quem escreve o conteúdo (o Markdown).** Sua política nativa funciona baseada em listas de permissões (*allowlists*). Por padrão, acessos a comandos do sistema operacional (`os/exec`), comunicações remotas e o uso de formatos de conteúdo como HTML puro (`allowContent`) são totalmente bloqueados para evitar ataques de injeção de scripts maliciosos (XSS). Ele só executa ferramentas externas que você autorizar explicitamente, como `tailwindcss` ou `postcss`. 

E como o resultado final publicado são apenas arquivos estáticos salvos em disco, **99% das vulnerabilidades tradicionais de servidores (como SQL Injection) deixam de existir.**

---

## Mudanças da versão 0.146.0 que me deram dor de cabeça

A equipe de desenvolvimento do Hugo refez completamente o sistema de processamento de templates na versão `v0.146.0`. Isso quebrou a compatibilidade com quase todos os tutoriais mais antigos da internet. Se você ler um tutorial que mande criar a pasta `_default` ou o arquivo `index.html`, saiba que ele está obsoleto.

Aqui está o que mudou e que você precisa aplicar no padrão moderno:
* **Fim da pasta `_default`:** Os layouts padrão agora moram diretamente na raiz da pasta `layouts/`.
* **Pastas de sistema com `_`:** Pastas internas do motor agora exigem um *underscore* na frente. Portanto, `partials/` virou `_partials/` e `shortcodes/` virou `_shortcodes/`.
* **Fim do `index.html` na Home:** O arquivo responsável por renderizar a página inicial agora deve obrigatoriamente se chamar `home.html`.
* **Nomenclatura de Base:** Identificadores de templates base mudaram o hífen pelo ponto. O antigo `list-baseof.html` agora deve ser renomeado para `baseof.list.html`.

Você pode conferir (e eu recomendo muito) as mudanças feitas nos templates na [Documentação Oficial](https://gohugo.io/templates/new-templatesystem-overview/).

---

## RSS no Hugo: Autonomia para o Leitor

O **RSS (Really Simple Syndication)** é o "Twitter dos tempos antigos" que ainda sobrevive como uma ferramenta fantástica. Ele gera um arquivo em formato `XML` contendo um resumo estruturado das suas novidades (título, link, data).

Com ele, o leitor não depende do algoritmo de nenhuma grande corporação tecnológica para ver seu conteúdo; basta ele assinar o seu feed em leitores como Feedly ou Inoreader para ser notificado na hora. Além disso, você pode plugar ferramentas como Zapier ou IFTTT no seu RSS para automatizar postagens automáticas no LinkedIn ou avisos no Discord sempre que um post sair. O Hugo gera esse arquivo (`index.xml`) de forma totalmente nativa.

---

## Globalizando o Blog com Multilingual Mode

O Hugo gerencia múltiplos idiomas de forma espetacular através de tabelas de tradução. Existem dois métodos para gerenciar seus arquivos: por **Sufixos** (ex: `post.en.md`, `post.pt-br.md`) ou por **Diretórios**.

### Sufixos vs. Diretórios: Aprenda com meu erro
Inicialmente, adotei o método de sufixos por parecer mais simples. Mas no momento em que decidi organizar meus posts em camadas e séries profundas (como `content/posts/Livros/Filosofia/`), misturar arquivos de cinco idiomas diferentes dentro da mesma pasta transformou o projeto em um caos absoluto.

**A Solução:** Migrei para a tradução por **Diretórios**. No `hugo.toml`, configurei pastas isoladas para cada língua (`content/pt-br/`, `content/en/`, etc.). Agora, os arquivos Markdown de todos os idiomas se chamam simplesmente `index.md`, mantendo a árvore de diretórios perfeitamente limpa. 

Para amarrar as páginas, usamos o atributo `translationKey` no cabeçalho do post. Não importa se a pasta em português se chama `Tudo-tem-um-inicio` e a em francês se chama `Tout-a-un-debut`; se ambas tiverem a mesma `translationKey`, o Hugo cria o vínculo no seletor de idiomas do menu de forma automática.

Para separar as strings de interface (como os botões "Voltar" ou "Buscar"), usamos a pasta `i18n/` com arquivos `.toml` para cada idioma, chamando-os no HTML através da função `{{ T "chave_da_string" }}`.

Nessa página da [Documentação Oficial do Hugo](https://gohugo.io/content-management/multilingual/) você consegue ler mais sobre como tratar diferentes idiomas.

---

## URLs Limpas: O Segredo dos Slugs e Permalinks

Por padrão, o Hugo reflete o caminho físico das suas pastas diretamente na URL do site. Isso gera links gigantes e redundantes. Pior ainda: se você mantiver o nome das suas pastas em português para sua própria organização mental, a sua URL em inglês acabaria herdando o termo em português.

Para contornar isso, usamos duas propriedades:
1.  **Slugs:** No *Front Matter* de cada arquivo, definimos o parâmetro `slug = "nome-limpo-da-url"`. Ele força a URL daquele post a adotar um termo específico e totalmente minúsculo.
2.  **Permalinks:** No arquivo `hugo.toml`, definimos uma regra global para centralizar as rotas, limpando os caminhos intermediários das pastas:

```toml
[permalinks]
  posts = "/posts/:slug/"
```

Isso garante que, não importa em qual subpasta o Markdown esteja escondido, a URL final gerada para o usuário será curta, elegante e amigável para SEO.

### GitHub Pages e GitHub Actions
Para colocar o blog no ar, fiz a união de duas ferramentas gratuitas do GitHub:

**GitHub Pages**: Funciona como a nossa hospedagem estática. Ele não processa linguagens de servidor; ele age apenas como uma vitrine que exibe os arquivos HTML, CSS e JS puros resultantes da compilação.

**GitHub Actions**: Como nosso repositório guarda apenas os arquivos de desenvolvimento (Markdowns e layouts), precisamos de alguém para compilar o site. O Actions é um computador virtual (um robô rodando Linux Ubuntu) que fica de prontidão. Sempre que eu dou um *git push main*, esse robô "acorda", lê nosso arquivo de instruções `hugo.yaml`, instala a versão correta do **Hugo Extended**, compila todo o projeto e entrega a pasta `public/` prontinha para o GitHub Pages exibir para o mundo.

## Mini Tutorial: Criando seu próprio blog com Hugo
Vamos colocar a mão na massa e criar um blog estruturado do zero, aplicando todas as regras modernas da versão 0.146.0+ (não tem porque não começar seu blog na versão mais atualizada).
Eu recomendo que você faça o tutorial com os exatos comandos que eu disponibilizei apenas para ver na prática como é criado um blog do zero. Depois disso, você pode refazer, adaptando os nomes
e tudo mais do jeito que você quiser.

### Passo 1: Preparando o Terreno (As Ferramentas)
Para que a nossa "fábrica" de páginas funcione, precisamos de três ferramentas:

**Git:** O sistema de versionamento de código.

**Go:** A linguagem base do nosso motor.

**Hugo (Versão Extended):** O nosso gerador de sites estáticos.



#### Opção A: Instalação via Terminal (Recomendado)
No Windows (PowerShell como Administrador):

PowerShell
```powershell
winget install Git.Git 
winget install GoLang.Go 
winget install Hugo.Hugo.Extended
```

No Linux (Ubuntu/Debian):

```bash
sudo apt update 
sudo apt install git golang hugo
```

No macOS (Homebrew):
```bash
brew install git go hugo
```

#### Opção B: Instalação Manual (Interface Gráfica)
Baixe e instale o Git e o Go clicando em **"Avançar"** nos instaladores padrão.

Para o Hugo, vá até as Releases no GitHub oficial do projeto e baixe a versão `.zip` correspondente ao seu sistema operacional (certifique-se de baixar a versão com extended no nome).

Extraia os arquivos em uma pasta segura (ex: `C:\Hugo\`).

No Windows: Procure no menu iniciar por "Editar as variáveis de ambiente do sistema". Clique em "Variáveis de Ambiente", dê um duplo clique em Path e adicione o caminho da pasta onde você colocou o Hugo.

Feche o terminal, abra-o novamente e digite hugo version. Se a versão aparecer, estamos prontos.

### Passo 2: A Fundação do Projeto

{{< figure src="Imagens-Tutorial-Hugo/01-VS-Code-Inicial.jpeg" title="Imagem inicial do VS Code (Editor de código) com a pasta do projeto aberta" alt="Imagem inicial do VS Code (Editor de código) com a pasta do projeto aberta." >}}

Abra o seu terminal na pasta onde deseja guardar o projeto e digite:

``` bash
hugo new site meu-blog
cd meu-blog
```

{{< figure src="Imagens-Tutorial-Hugo/02-Criando-o-site.jpeg" title="Imagem inicial dos primeiros comandos no terminal do VS Code que inicializam o projeto do blog feito com Hugo" alt="Imagem inicial dos primeiros comandos no terminal do VS Code que inicializam o projeto do blog feito com Hugo." >}}

**Atenção:** O comando `cd meu-blog` é crucial. Ele garante que o seu terminal esteja dentro da pasta do projeto. Todos os próximos passos devem ser executados a partir daqui.

### Passo 3: Configurando a Base e o Multilinguismo
Vamos preparar o projeto para suportar múltiplos idiomas usando a metodologia de Diretórios.

Abra o arquivo `hugo.toml` e substitua tudo por isso (não esqueça de trocar a baseURL pelo seu nome de usuário do GitHub):

```toml
baseURL = "https://SEU-USUARIO.github.io/meu-blog/"
title = "Meu Blog Profissional"
defaultContentLanguage = "pt-br"

[permalinks]
  posts = "/posts/:slug/"

[languages]
  [languages."pt-br"]
    languageName = "Português"
    weight = 1
    contentDir = "content/pt-br"
```
{{< figure src="Imagens-Tutorial-Hugo/04-Hugo-TOML.jpeg" title="Arquivo hugo.toml adaptado" alt="Arquivo hugo.toml adaptado." >}}


No terminal, crie as pastas base para os conteúdos em português:

```bash
mkdir -p content/pt-br/posts
mkdir -p content/pt-br/about
```

{{< figure src="Imagens-Tutorial-Hugo/05-Pastas-Base-PTBR.jpeg" title="Comandos para criar as pastas base para os conteúdos em português" alt="Comandos para criar as pastas base para os conteúdos em português." >}}
{{< figure src="Imagens-Tutorial-Hugo/06-Estruturas-Pastas-Base-PTBR.jpeg" title="Sua estrutura de pastas deve estar assim" alt="Imagem da estrutura de pastas após os últimos comandos." >}}


### Passo 4: Construindo o Layout (A Fôrma)
Vamos criar as estruturas HTML que vão receber os nossos textos, já embutindo a lógica de Dark Mode e o Sumário Lateral. Crie os arquivos abaixo dentro da pasta `layouts/`:

**1. O Esqueleto (layouts/baseof.html):**

```html
<!DOCTYPE html>
<html lang="{{ .Site.Language.Lang }}">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .Title }} | {{ .Site.Title }}</title>
    <link rel="stylesheet" href="{{ "css/custom.css" | relURL }}">
    
    <script>
        function toggleTheme() {
            document.documentElement.classList.toggle('dark');
            localStorage.setItem('theme', document.documentElement.classList.contains('dark') ? 'dark' : 'light');
        }
        if (localStorage.getItem('theme') === 'dark' || (!localStorage.getItem('theme') && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
            document.documentElement.classList.add('dark');
        }
    </script>
</head>
<body>
    <header class="site-header">
        <div class="header-container">
            <h1><a href="{{ .Site.Home.RelPermalink }}">{{ .Site.Title }}</a></h1>
            <nav class="site-nav">
                <a href="{{ "/posts/" | relLangURL }}">Artigos</a>
                <a href="{{ "/about/" | relLangURL }}">Sobre</a>
                <button onclick="toggleTheme()" class="theme-btn" aria-label="Mudar Tema">🌓</button>
            </nav>
        </div>
    </header>
    
    <main>
        {{ block "main" . }}{{ end }}
    </main>
    
    <footer class="site-footer">
        <p>© 2026 {{ .Site.Title }}.</p>
    </footer>
</body>
</html>
```

{{< figure src="Imagens-Tutorial-Hugo/07-baseof-html.jpeg" title="Sua estrutura de pastas deve estar assim" alt="Imagem da estrutura de pastas após os últimos comandos." >}}


**2. A Página Inicial (layouts/home.html):**

```html
{{ define "main" }}
<div class="container hero">
    <h2>Bem-vindo!</h2>
    <p>Um espaço minimalista para documentar ideias e aprendizados.</p>
</div>
{{ end }}
```
{{< figure src="Imagens-Tutorial-Hugo/08-home-html.jpeg" title="Conteúdo do arquivo home.html e estrutura de pastas" alt="Conteúdo do arquivo home.html e estrutura de pastas." >}}


**3. O Post Único e a Página Sobre (layouts/single.html):**
Aqui inserimos o TableOfContents para gerar o índice dinâmico.

```html
{{ define "main" }}
<div class="post-layout-container">
    <article class="post-main">
        <header class="post-header">
            <h1>{{ .Title }}</h1>
            {{ if not .Params.isAbout }}
            <time>{{ .Date.Format "02/01/2006" }}</time>
            {{ end }}
        </header>
        <div class="post-content">
            {{ .Content }}
        </div>
    </article>

    <aside class="post-toc">
        <div class="toc-sticky">
            <span class="toc-title">Neste Post</span>
            {{ .TableOfContents }}
        </div>
    </aside>
</div>
{{ end }}
```
{{< figure src="Imagens-Tutorial-Hugo/09-single-html.jpeg" title="Conteúdo do arquivo single.html e estrutura de pastas" alt="Conteúdo do arquivo single.html e estrutura de pastas." >}}

**4. A Lista de Posts (layouts/list.html):**

```html
{{ define "main" }}
<div class="container">
    <h2>{{ .Title }}</h2>
    <ul class="post-list">
        {{ range .Pages }}
        <li>
            <time>{{ .Date.Format "02/01/2006" }}</time>
            <a href="{{ .RelPermalink }}">{{ .Title }}</a>
        </li>
        {{ end }}
    </ul>
</div>
{{ end }}
```
{{< figure src="Imagens-Tutorial-Hugo/10-list-html.jpeg" title="Conteúdo do arquivo list.html e estrutura de pastas" alt="Conteúdo do arquivo list.html e estrutura de pastas." >}}

### Passo 5: A Pintura (O CSS Profissional)
O Hugo gerou automaticamente uma pasta `static/` na raiz do projeto. Entre nela, crie uma subpasta `css/` e um arquivo chamado `custom.css`.

Abra `static/css/custom.css` e cole este código. Ele possui variáveis automáticas para o Modo Escuro e estiliza a barra lateral.

```css

:root {
    --bg-color: #FAFAFA;
    --text-main: #1A1A1A;
    --text-muted: #666666;
    --accent: #2B6CB0;
    --border: #E2E8F0;
}

html.dark {
    --bg-color: #1A202C;
    --text-main: #F7FAFC;
    --text-muted: #A0AEC0;
    --accent: #63B3ED;
    --border: #2D3748;
}

body {
    background-color: var(--bg-color);
    color: var(--text-main);
    font-family: 'Segoe UI', system-ui, sans-serif;
    line-height: 1.6;
    margin: 0;
    transition: background-color 0.3s, color 0.3s;
}

a { color: var(--accent); text-decoration: none; }
a:hover { text-decoration: underline; }

.container { max-width: 800px; margin: 0 auto; padding: 2rem 1.5rem; }

.site-header { border-bottom: 1px solid var(--border); padding: 1rem 1.5rem; }
.header-container { max-width: 1080px; margin: 0 auto; display: flex; justify-content: space-between; align-items: center; }
.header-container h1 { margin: 0; font-size: 1.25rem; }
.header-container a { color: var(--text-main); text-decoration: none; }
.site-nav { display: flex; gap: 1.5rem; align-items: center; }
.theme-btn { background: none; border: none; font-size: 1.2rem; cursor: pointer; }

.post-layout-container {
    max-width: 1080px;
    margin: 0 auto;
    display: flex;
    align-items: stretch;
    justify-content: center;
    gap: 4rem;
    padding: 2rem 1.5rem;
}

.post-main { flex: 1; max-width: 720px; }
.post-header { border-bottom: 1px solid var(--border); margin-bottom: 2rem; padding-bottom: 1rem; }
.post-header h1 { margin: 0 0 0.5rem 0; font-size: 2.2rem; }
.post-header time { color: var(--text-muted); font-size: 0.9rem; }

.post-content h2 { margin-top: 2rem; }
.post-content p { margin-bottom: 1.2rem; }

.post-toc { width: 240px; flex-shrink: 0; display: none; padding-top: 1rem; }
.toc-sticky {
    position: sticky; top: 2rem; max-height: calc(100vh - 4rem);
    overflow-y: auto; scrollbar-width: none;
}
.toc-sticky::-webkit-scrollbar { display: none; }
.toc-title { font-weight: bold; text-transform: uppercase; font-size: 0.8rem; color: var(--text-muted); display: block; margin-bottom: 1rem; }
#TableOfContents ul { list-style: none; padding: 0; margin: 0; }
#TableOfContents ul ul { padding-left: 1rem; border-left: 1px solid var(--border); margin-left: 0.5rem; margin-top: 0.5rem; }
#TableOfContents li { margin-bottom: 0.5rem; }
#TableOfContents a { color: var(--text-muted); font-size: 0.9rem; }
#TableOfContents a:hover { color: var(--text-main); text-decoration: none; }

.post-list { list-style: none; padding: 0; }
.post-list li { margin-bottom: 1rem; }
.post-list time { color: var(--text-muted); margin-right: 1rem; font-size: 0.9rem; }
.site-footer { text-align: center; padding: 2rem; border-top: 1px solid var(--border); color: var(--text-muted); font-size: 0.9rem; }

@media (min-width: 1024px) {
    .post-toc { display: block; }
}
```
{{< figure src="Imagens-Tutorial-Hugo/11-css-custom.jpeg" title="Conteúdo do arquivo custom.css e estrutura de pastas" alt="Conteúdo do arquivo custom.css e estrutura de pastas." >}}

### Passo 6: Criando os Conteúdos
**1. A Página "Sobre"**:
No terminal, crie a página Sobre:

```bash
hugo new content content/pt-br/about/index.md
```
{{< figure src="Imagens-Tutorial-Hugo/12-about-page.jpeg" title="Comandos para criar a página Sobre" alt="Comandos para criar a página Sobre" >}}


Edite o arquivo recém-criado em content/pt-br/about/index.md:

```markdown
---
title: "Sobre mim"
slug: "about"
isAbout: true
translationKey: "about"
---

Olá! Sou um desenvolvedor compartilhando minha jornada, estudos e ideias neste blog.
```

(Nota: usamos `isAbout: true` para o nosso layout HTML saber que não deve exibir a data nessa página).

{{< figure src="Imagens-Tutorial-Hugo/13-index-about.jpeg" title="Estado inicial do index da página Sobre" alt="Estado inicial do index da página Sobre" >}}

{{< figure src="Imagens-Tutorial-Hugo/14-pagina-sobre-editada.jpeg" title="Página Sobre após alterações" alt="Página Sobre após alterações" >}}

**2. O Primeiro Post:**

```bash
hugo new content content/pt-br/posts/meu-primeiro-artigo/index.md
```
{{< figure src="Imagens-Tutorial-Hugo/15-my-first-article.jpeg" title="Comando para criar o arquivo do primeiro artigo" alt="Comando para criar o arquivo do primeiro artigo" >}}

{{< figure src="Imagens-Tutorial-Hugo/16-estado-inicial-primeiro-artigo-md.jpeg" title="Estado inicial do markdown de primeiro artigo" alt="Estado inicial do markdown de primeiro artigo" >}}

Edite o arquivo em content/pt-br/posts/meu-primeiro-artigo/index.md:

```markdown
---
title: "Meu Primeiro Artigo"
slug: "meu-primeiro-artigo"
date: 2026-06-09T10:00:00-03:00
draft: false
translationKey: "primeiro-artigo"
---

Abaixo, exploramos as vantagens de gerar sites estáticos.

## Velocidade Absoluta
O Hugo compila páginas em milissegundos.

## Segurança
Como não há banco de dados rodando em tempo real, as chances de invasão despencam.
```

{{< figure src="Imagens-Tutorial-Hugo/17-primeiro-artigo-pos-alteracoes.jpeg" title="Estado do markdown de primeiro artigo após alterações" alt="Estado do markdown de primeiro artigo após alterações" >}}


No terminal, suba o servidor:

```bash
hugo server
```
{{< figure src="Imagens-Tutorial-Hugo/18-hugo-server-comando.jpeg" title="Comando hugo server no terminal" alt="Comando hugo server no terminal" >}}

Abra o navegador em `http://localhost:1313`. Você irá ver o modelo de blog pronto. O visual está bem simples, mas não se preocupe, você pode alterar isso depois.

{{< figure src="Imagens-Tutorial-Hugo/19-modelo-blog-pronto.jpeg" title="Modelo do blog pronto" alt="Modelo do blog pronto" >}}

### Passo 7: O Deploy Gratuito (GitHub Actions)

Vamos colocar isso no ar de graça.

Antes de enviarmos o código, precisamos avisar o Git para não salvar arquivos temporários ou a pasta do site final (já que o robô do GitHub fará isso por nós). Crie um arquivo chamado `.gitignore` na raiz do seu projeto e cole o seguinte:
```plaintext
public/
resources/
.hugo_build.lock
```

Só para você entender cada linha:

`public/`: Esta é a pasta onde o Hugo joga os arquivos HTML estáticos finais quando você roda o comando de compilação. Como nós usamos o GitHub Actions para compilar isso direto no servidor deles, não precisamos enviar a pasta gerada no nosso computador.

`resources/`: O Hugo cria essa pasta como um cache temporário para processar imagens e estilos de forma mais rápida nas próximas vezes. Ela não precisa ser versionada.

`.hugo_build.lock`: É um pequeno arquivo de trava de segurança criado pelo Hugo para impedir que duas instâncias rodem o build ao mesmo tempo. Não serve para nada no repositório.

Crie um repositório vazio no GitHub clicando no botão verde.

{{< figure src="Imagens-Tutorial-Hugo/19.1-new-repository.png" title="Botão New" alt="Botão New para criar novo repositório no GitHub" >}}

Não esqueça de configurar se você quer que seu repositório fique público ou privado. Se ficar público, qualquer pessoa pode acessar os arquivos que você enviar para esse repositório no GitHub. Se ficar privado, somente você ou quem tiver permissão pode vê-los.

{{< figure src="Imagens-Tutorial-Hugo/20-github-repositorio.jpeg" title="Repositório sendo criado no GitHub" alt="Repositório sendo criado no GitHub" >}}

No seu terminal (ainda dentro da pasta meu-blog), inicie o Git:

```bash
git init
git add .
git commit -m "commit inicial"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
git push -u origin main
```

{{< figure src="Imagens-Tutorial-Hugo/21-git-init.jpeg" title="Comandos para iniciar o Git" alt="Comandos para iniciar o Git" >}}

{{< figure src="Imagens-Tutorial-Hugo/22-commit-inicial.jpeg" title="Comandos para o primeiro salvamento de código no GitHub" alt="Comandos para o primeiro salvamento de código no GitHub" >}}

{{< figure src="Imagens-Tutorial-Hugo/23-git-remote.jpeg" title="Comando para mudar o nome da sua branch (convenção) e conectar o seu terminal ao repositório no GitHub" alt="Comando para mudar o nome da sua branch (convenção) e conectar o seu terminal ao repositório no GitHub" >}}

{{< figure src="Imagens-Tutorial-Hugo/24-primeiro-push.jpeg" title="Comando para enviar seu código ao GitHub pela primeira vez" alt="Comando para enviar seu código ao GitHub pela primeira vez" >}}


No GitHub, vá na aba **Settings > Pages**. Em **Source**, escolha **GitHub Actions**.
{{< figure src="Imagens-Tutorial-Hugo/25-github-settings.jpeg" title="Acessar Settings ou Configuração no GitHub a partir do repositório do seu projeto" alt="Indicação de onde acessar Settings ou Configuração no GitHub a partir do repositório do seu projeto" >}}

{{< figure src="Imagens-Tutorial-Hugo/26-source-github-pages.jpeg" title="Definir Source para usar GitHub Actions" alt="Indicação de onde definir Source para usar GitHub Actions" >}}

Volte ao seu editor de código. Dentro da pasta do seu projeto (meu-blog), crie uma pasta chamada `.github`. Dentro dela, crie outra chamada `workflows`, e lá dentro crie um arquivo chamado `hugo.yaml` (o caminho final exato deve ser `meu-blog/.github/workflows/hugo.yaml`).

```yaml
name: Deploy Hugo site to Pages
on:
  push:
    branches: ["main"]
  workflow_dispatch:
permissions:
  contents: read
  pages: write
  id-token: write
concurrency:
  group: "pages"
  cancel-in-progress: false
defaults:
  run:
    shell: bash
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: 'latest'
          extended: true
      - name: Build
        run: hugo --minify
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

{{< figure src="Imagens-Tutorial-Hugo/27-workflows.jpeg" title="Estrutura de pastas: pasta workflows dentro de pasta .github com arquivo hugo.yaml" alt="Estrutura de pastas: pasta workflows dentro de pasta .github com arquivo hugo.yaml" >}}

Faça um `git add .`no terminal, depois o `commit` e o `push`. O robô vai "acordar", compilar seu site e deixá-lo público no seu endereço oficial.
{{< figure src="Imagens-Tutorial-Hugo/28-segundo-push.jpeg" title="Comandos para enviar as alterações para o repositório do GitHub" alt="Comandos para enviar as alterações para o repositório do GitHub" >}}

Se você ir na aba **Actions** do GitHub, pode ver o processo de *build* dele rodando (quando estiver em amarelo) ou concluído (quando estiver em verde).

{{< figure src="Imagens-Tutorial-Hugo/30-GitHubActions-building.jpeg" title="GitHub Actions com build em andamento" alt="GitHub Actions com build em andamento" >}}

{{< figure src="Imagens-Tutorial-Hugo/31-GitHubActions-Estado-Final.png" title="GitHub Actions com build finalizado" alt="GitHub Actions com o build finalizado" >}}

Cada vez que você fizer um novo *git push* para enviar as alterações ao GitHub (como por exemplo, quando você finaliza um novo arquivo markdown, que é um post), o GitHub Actions cuida de colocar seu blog ao ar na versão mais atualizada. Só não esqueça de usar *git add* para adicionar os arquivos ao que chamamos de Staging antes e *git commit* para salvar as alterações antes de enviar ao GitHub (senão você não irá enviar nada).

Após concluir esse tutorial, você deve ter uma estrutura inicial bem simples. Sugiro que você peça para uma IA te ajudar a deixar isso mais bonito e adaptar do jeito que você quer. O importante, que é a estrutura, já foi feito.


## Recursos úteis

Você pode conferir a [Documentação Oficial do Hugo](https://gohugo.io/) para aprender mais. Você vai encontrar muitas informações importantes ao acessar a aba "Docs".

Um vídeo que me ajudou muito a começar a aprender sobre o Hugo foi esse vídeo do **Fireship** (conteúdo em inglês):
{{< youtube 0RKpf3rK57I >}}


Eu também recomendo o vídeo do **NetworkChuck** (conteúdo em inglês), onde ele ensina a criar um blog com Hugo, aplicar um tema e usar o **Obsidian** para escrever os posts.
{{< youtube dnE7c0ELEH8 >}}


## Finalização

Como eu já mencionei no primeiro post que fiz para esse blog, um dos meus objetivos é melhorar minha escrita de forma progressiva. Sei que esse artigo não está nem perto do nível que eu quero que fique, mas acho que já é uma introdução ok para quem busca aprender sobre o Hugo. Incentivo que você leia a documentação oficial, pois é lá que você irá encontrar tudo que precisa. Se você não sabe inglês, pode utilizar ferramentas como Google Translate, ou ainda melhor, pedir que uma IA forneça a você explicações da documentação traduzidas para seu idioma nativo.

Eu ainda sei muito pouco sobre o Hugo e estou aprendendo mais conforme vou escrevendo esses posts. Se me der vontade, talvez eu traga mais posts falando sobre funcionalidades e coisas legais que podemos fazer com essa ferramenta, como por exemplo a lógica da Table of Contents (esse menu de navegação por títulos da página) que eu implementei no meu próprio blog conforme escrevia esse post.
