---
title: "Everything Has a Beginning — Part 2"
slug: "everything-has-a-beginning-part-2"
date: 2026-06-09T10:12:00-03:00
draft: false
categories: ["Beginning"]
series: ["Everything Has a Beginning"]
description: "The second part of building MitzIdeas, focusing on installing and configuring Hugo."
translationKey: "primeiro-post-parte-2"
---

## Introduction

In the previous post, I shared the idea behind the birth of this blog. Today, we're going to open the hood and talk about the engineering that keeps MitzIdeas running without costing me a single cent in hosting, while maintaining absurd speed and full control over every pixel.

I'll try to talk a bit more about the entire structure that holds this blog up, explain each concept more clearly, and at the end offer a mini tutorial to help you go from zero to your first Hugo-powered blog.

---

## The Engine: Understanding Hugo

Hugo is a **Static Site Generator (SSG)** written in the **Go** programming language. It was created by **Steve Francia** (known as *spf13* in the Golang ecosystem) in 2013, who designed the base structure for how the project processes templates and the famous *lookup order* logic.

Around 2015, leadership was handed over to **Bjorn Erik Pedersen** (*bep*), who transformed this engine into a true professional race car. Among Bjorn's most notable achievements:

* **Extreme Performance:** Compiler optimization to make the most of multi-core processors, allowing sites with tens of thousands of pages to be compiled in just a few seconds.
* **Hugo Pipes:** Native asset processing, minification, fingerprinting, and robust multilingual (`i18n`) support.
* **Community:** An almost superhuman dedication to answering issues and reviewing pull requests on GitHub, keeping the project at the cutting edge of the Web.

Because of this flexible framework, Hugo is widely used to create documentation portals, government sites, corporate sites, and yes, personal blogs. It acts as an excellent file-based **Content Management System (CMS)**, where your code editor (like VS Code) becomes your control panel.

---

## Why use Hugo instead of plain HTML, CSS, and JavaScript?

If you build a site with pure HTML, CSS, and JS (the famous *Vanilla* approach), you're laying bricks one by one. It works beautifully for 3 or 5 pages. But what about when you have 100 posts?

Imagine I have 50 posts and decide to change the "About" link in the header:
* **In plain HTML:** I'd have to open **51 files** (the 50 posts + the `index.html` page) and change them manually one by one. Miss one and you've got an inconsistency.
* **In Hugo:** I change a single partial file (`layouts/_partials/header.html`) and, when the build runs, Hugo injects that change across every page instantly. Hugo is a **page factory**.

On top of that, writing in Markdown (`.md`) is infinitely more productive than assembling HTML tags like LEGO bricks. You focus on the text, and the engine handles the rest.

The ecosystem also brings advantages like **Minification** (stripping whitespace from code) and **Fingerprinting** (a cache busting technique that adds a unique hash to filenames, like `custom.min.8723.css`, forcing the user's browser to download the newest version and ignore the old cache).

### When NOT to use Hugo?
I'm not here to sell Hugo as a silver bullet. It's overkill for simple single-page landing pages. It's also not ideal for systems that require real-time server-side data processing, like user-authenticated dashboards or dynamic social media feeds. In those scenarios, frameworks like Django (which I use day-to-day) remain the right choice.

---

## Core Architectural Features

### 1. Built-in Development Server
When you type `hugo server` in the terminal, it spins up a local server that creates a "tunnel" between your files and the browser. Every time you save a file, it detects the changes and reloads the page in milliseconds.

Since Hugo is built in Go, we have access to **Go Templates** for interpolating and transforming dynamic data. Anyone coming from the Python/Django ecosystem will notice an immediate similarity. It's a fully *batteries included* environment.

### 2. Layout Hierarchy
Hugo works with molds. You define a main file called `baseof.html` with the site's skeleton (everything that repeats, like the header and footer) and open a "hole" using the `{{ block "main" . }}{{ end }}` directive. Other templates simply inherit that mold and fill in the hole.

### 3. Native Taxonomies
Classifying content in Hugo is extremely powerful and requires no plugins. The **taxonomy** system creates complex logical relationships (like tags, categories, and series), generating a weighted inverted index. It knows exactly which posts are related to each other without needing a single SQL query.

### 4. Dictionary of Unusual Files
* `archetypes/`: Stores your draft templates. The `default.md` file defines which fields come pre-filled every time you create a new post.
* `.toml`: The configuration file format (like `hugo.toml`). It's the site's global brain.
* `single.html` vs `list.html`: The former dictates the layout of a single post; the latter renders pages that list multiple posts (like category indexes).

---

## The Security Model: "Restricted Trust"

Hugo operates under a very clear premise: **it trusts the person writing the code (the developer), but not the person writing the content (the Markdown).** Its native policy is based on allowlists. By default, access to OS commands (`os/exec`), remote communications, and the use of raw content formats like plain HTML (`allowContent`) are completely blocked to prevent malicious script injection attacks (XSS). It only runs external tools you explicitly authorize, like `tailwindcss` or `postcss`.

And since the final published output is just static files saved to disk, **99% of traditional server vulnerabilities (like SQL Injection) simply cease to exist.**

---

## Breaking Changes in v0.146.0 That Gave Me a Headache

The Hugo development team completely rebuilt the template processing system in version `v0.146.0`. This broke compatibility with almost every older tutorial on the internet. If you find a tutorial telling you to create a `_default` folder or an `index.html` file, know that it's outdated.

Here's what changed and what you need to apply in the modern pattern:
* **End of the `_default` folder:** Default layouts now live directly in the root of the `layouts/` folder.
* **System folders with `_`:** Internal engine folders now require a leading underscore. So `partials/` became `_partials/` and `shortcodes/` became `_shortcodes/`.
* **End of `index.html` for the Home:** The file responsible for rendering the homepage must now be named `home.html`.
* **Base Naming:** Template base identifiers switched from hyphens to dots. The old `list-baseof.html` must now be renamed to `baseof.list.html`.

You can check out (and I highly recommend) the changes made to the template system in the [Official Documentation](https://gohugo.io/templates/new-templatesystem-overview/).

---

## RSS in Hugo: Reader Autonomy

**RSS (Really Simple Syndication)** is the "old-school Twitter" that still survives as a fantastic tool. It generates an `XML` file containing a structured summary of your latest content (title, link, date).

With it, readers don't depend on any big tech company's algorithm to see your content; they just subscribe to your feed in readers like Feedly or Inoreader and get notified immediately. You can also plug tools like Zapier or IFTTT into your RSS to automate posts to LinkedIn or Discord notifications whenever something goes live. Hugo generates this file (`index.xml`) entirely natively.

---

## Globalizing the Blog with Multilingual Mode

Hugo handles multiple languages spectacularly through translation tables. There are two methods for managing your files: by **Suffixes** (e.g., `post.en.md`, `post.pt-br.md`) or by **Directories**.

### Suffixes vs. Directories: Learn From My Mistake
Initially, I went with the suffix method because it seemed simpler. But the moment I decided to organize my posts into deep layers and series (like `content/posts/Books/Philosophy/`), mixing files from five different languages inside the same folder turned the project into absolute chaos.

**The Solution:** I migrated to **Directory**-based translation. In `hugo.toml`, I configured isolated folders for each language (`content/pt-br/`, `content/en/`, etc.). Now, the Markdown files for all languages are simply named `index.md`, keeping the directory tree perfectly clean.

To link the pages together, we use the `translationKey` attribute in the post's Front Matter. It doesn't matter if the Portuguese folder is called `Tudo-tem-um-inicio` and the French one is `Tout-a-un-debut`; if both share the same `translationKey`, Hugo creates the link in the language selector menu automatically.

To handle UI strings (like "Back" or "Search" buttons), we use the `i18n/` folder with `.toml` files for each language, calling them in HTML through the `{{ T "string_key" }}` function.

You can read more about handling multiple languages on the [Hugo Official Documentation](https://gohugo.io/content-management/multilingual/) page.

---

## Clean URLs: The Secret of Slugs and Permalinks

By default, Hugo mirrors the physical path of your folders directly into the site's URL. This generates long, redundant links. Worse yet: if you keep your folder names in Portuguese for your own mental organization, your English URL would end up inheriting the Portuguese term.

To work around this, we use two properties:
1. **Slugs:** In each file's Front Matter, we define the `slug = "clean-url-name"` parameter. It forces that post's URL to adopt a specific, fully lowercase term.
2. **Permalinks:** In the `hugo.toml` file, we define a global rule to centralize routes, cleaning up the intermediate folder paths:

```toml
[permalinks]
  posts = "/posts/:slug/"
```

This guarantees that no matter which subfolder the Markdown is nested in, the final URL generated for the user will be short, elegant, and SEO-friendly.

### GitHub Pages and GitHub Actions
To get the blog live, I combined two free GitHub tools:

**GitHub Pages**: Acts as our static hosting. It doesn't process server-side languages; it simply acts as a storefront displaying the pure HTML, CSS, and JS files resulting from the build.

**GitHub Actions**: Since our repository only stores the development files (Markdowns and layouts), we need something to compile the site. Actions is a virtual machine (a robot running Linux Ubuntu) that stands by on call. Whenever I do a *git push main*, that robot "wakes up", reads our instructions file `hugo.yaml`, installs the correct version of **Hugo Extended**, compiles the entire project, and delivers the `public/` folder ready for GitHub Pages to display to the world.

## Mini Tutorial: Creating Your Own Hugo Blog
Let's get our hands dirty and create a structured blog from scratch, applying all the modern rules from version 0.146.0+ (there's no reason not to start your blog on the most up-to-date version).
I recommend you follow the tutorial with the exact commands I've provided just to see in practice how a blog is created from zero with Hugo. After that, you can redo it, adapting the names and everything else however you like.

### Step 1: Preparing the Ground (The Tools)
For our page "factory" to work, we need three tools:

**Git:** The code versioning system.

**Go:** The base language of our engine.

**Hugo (Extended Version):** Our static site generator.



#### Option A: Installation via Terminal (Recommended)
On Windows (PowerShell as Administrator):

PowerShell
```powershell
winget install Git.Git 
winget install GoLang.Go 
winget install Hugo.Hugo.Extended
```

On Linux (Ubuntu/Debian):

```bash
sudo apt update 
sudo apt install git golang hugo
```

On macOS (Homebrew):
```bash
brew install git go hugo
```

#### Option B: Manual Installation (Graphical Interface)
Download and install Git and Go by clicking "Next" through the standard installers.

For Hugo, go to the official project's GitHub Releases page and download the `.zip` version corresponding to your operating system (make sure to download the version with *extended* in the name).

Extract the files to a safe folder (e.g., `C:\Hugo\`).

On Windows: Search in the Start Menu for "Edit the system environment variables". Click "Environment Variables", double-click on Path, and add the path to the folder where you placed Hugo.

Close the terminal, reopen it, and type `hugo version`. If the version appears, we're ready to go.

### Step 2: The Project Foundation

{{< figure src="Imagens-Tutorial-Hugo/01-VS-Code-Inicial.jpeg" title="Initial VS Code (code editor) view with the project folder open" alt="Initial VS Code (code editor) view with the project folder open." >}}

Open your terminal in the folder where you want to store the project and type:

``` bash
hugo new site meu-blog
cd meu-blog
```

{{< figure src="Imagens-Tutorial-Hugo/02-Criando-o-site.jpeg" title="Initial view of the first terminal commands in VS Code that initialize the Hugo blog project" alt="Initial view of the first terminal commands in VS Code that initialize the Hugo blog project." >}}

**Note:** The `cd meu-blog` command is crucial. It ensures your terminal is inside the project folder. All the next steps must be run from here.

### Step 3: Configuring the Base and Multilingualism
Let's prepare the project to support multiple languages using the Directory methodology.

Open the `hugo.toml` file and replace everything with this (don't forget to swap the baseURL for your GitHub username):

```toml
baseURL = "https://YOUR-USERNAME.github.io/meu-blog/"
title = "My Professional Blog"
defaultContentLanguage = "pt-br"

[permalinks]
  posts = "/posts/:slug/"

[languages]
  [languages."pt-br"]
    languageName = "Português"
    weight = 1
    contentDir = "content/pt-br"
```
{{< figure src="Imagens-Tutorial-Hugo/04-Hugo-TOML.jpeg" title="Adapted hugo.toml file" alt="Adapted hugo.toml file." >}}


In the terminal, create the base folders for Portuguese content:

```bash
mkdir -p content/pt-br/posts
mkdir -p content/pt-br/about
```

{{< figure src="Imagens-Tutorial-Hugo/05-Pastas-Base-PTBR.jpeg" title="Commands to create the base folders for Portuguese content" alt="Commands to create the base folders for Portuguese content." >}}
{{< figure src="Imagens-Tutorial-Hugo/06-Estruturas-Pastas-Base-PTBR.jpeg" title="Your folder structure should look like this" alt="Image of the folder structure after the last commands." >}}


### Step 4: Building the Layout (The Mold)
Let's create the HTML structures that will receive our text, already embedding Dark Mode logic and a Sidebar Table of Contents. Create the files below inside the `layouts/` folder:

**1. The Skeleton (layouts/baseof.html):**

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
                <a href="{{ "/posts/" | relLangURL }}">Articles</a>
                <a href="{{ "/about/" | relLangURL }}">About</a>
                <button onclick="toggleTheme()" class="theme-btn" aria-label="Toggle Theme">🌓</button>
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

{{< figure src="Imagens-Tutorial-Hugo/07-baseof-html.jpeg" title="Your folder structure should look like this" alt="Image of the folder structure after the last commands." >}}


**2. The Homepage (layouts/home.html):**

```html
{{ define "main" }}
<div class="container hero">
    <h2>Welcome!</h2>
    <p>A minimalist space to document ideas and learnings.</p>
</div>
{{ end }}
```
{{< figure src="Imagens-Tutorial-Hugo/08-home-html.jpeg" title="Content of the home.html file and folder structure" alt="Content of the home.html file and folder structure." >}}


**3. The Single Post and About Page (layouts/single.html):**
Here we insert the TableOfContents to generate the dynamic index.

```html
{{ define "main" }}
<div class="post-layout-container">
    <article class="post-main">
        <header class="post-header">
            <h1>{{ .Title }}</h1>
            {{ if not .Params.isAbout }}
            <time>{{ .Date.Format "01/02/2006" }}</time>
            {{ end }}
        </header>
        <div class="post-content">
            {{ .Content }}
        </div>
    </article>

    <aside class="post-toc">
        <div class="toc-sticky">
            <span class="toc-title">In This Post</span>
            {{ .TableOfContents }}
        </div>
    </aside>
</div>
{{ end }}
```
{{< figure src="Imagens-Tutorial-Hugo/09-single-html.jpeg" title="Content of the single.html file and folder structure" alt="Content of the single.html file and folder structure." >}}

**4. The Post List (layouts/list.html):**

```html
{{ define "main" }}
<div class="container">
    <h2>{{ .Title }}</h2>
    <ul class="post-list">
        {{ range .Pages }}
        <li>
            <time>{{ .Date.Format "01/02/2006" }}</time>
            <a href="{{ .RelPermalink }}">{{ .Title }}</a>
        </li>
        {{ end }}
    </ul>
</div>
{{ end }}
```
{{< figure src="Imagens-Tutorial-Hugo/10-list-html.jpeg" title="Content of the list.html file and folder structure" alt="Content of the list.html file and folder structure." >}}

### Step 5: The Paint (The Professional CSS)
Hugo automatically generated a `static/` folder in the project root. Go into it, create a `css/` subfolder, and create a file called `custom.css`.

Open `static/css/custom.css` and paste this code. It has automatic variables for Dark Mode and styles the sidebar.

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
{{< figure src="Imagens-Tutorial-Hugo/11-css-custom.jpeg" title="Content of the custom.css file and folder structure" alt="Content of the custom.css file and folder structure." >}}

### Step 6: Creating the Content
**1. The "About" Page**:
In the terminal, create the About page:

```bash
hugo new content content/pt-br/about/index.md
```
{{< figure src="Imagens-Tutorial-Hugo/12-about-page.jpeg" title="Commands to create the About page" alt="Commands to create the About page" >}}


Edit the newly created file at content/pt-br/about/index.md:

```markdown
---
title: "About Me"
slug: "about"
isAbout: true
translationKey: "about"
---

Hi! I'm a developer sharing my journey, studies, and ideas on this blog.
```

(Note: we use `isAbout: true` so our HTML layout knows it should not display the date on this page.)

{{< figure src="Imagens-Tutorial-Hugo/13-index-about.jpeg" title="Initial state of the About page index" alt="Initial state of the About page index" >}}

{{< figure src="Imagens-Tutorial-Hugo/14-pagina-sobre-editada.jpeg" title="About page after edits" alt="About page after edits" >}}

**2. The First Post:**

```bash
hugo new content content/pt-br/posts/meu-primeiro-artigo/index.md
```
{{< figure src="Imagens-Tutorial-Hugo/15-my-first-article.jpeg" title="Command to create the first article file" alt="Command to create the first article file" >}}

{{< figure src="Imagens-Tutorial-Hugo/16-estado-inicial-primeiro-artigo-md.jpeg" title="Initial state of the first article markdown" alt="Initial state of the first article markdown" >}}

Edit the file at content/pt-br/posts/meu-primeiro-artigo/index.md:

```markdown
---
title: "My First Article"
slug: "my-first-article"
date: 2026-06-09T10:00:00-03:00
draft: false
translationKey: "primeiro-artigo"
---

Below, we explore the advantages of generating static sites.

## Absolute Speed
Hugo compiles pages in milliseconds.

## Security
Since there's no database running in real time, the chances of intrusion drop dramatically.
```

{{< figure src="Imagens-Tutorial-Hugo/17-primeiro-artigo-pos-alteracoes.jpeg" title="State of the first article markdown after edits" alt="State of the first article markdown after edits." >}}


In the terminal, start the server:

```bash
hugo server
```
{{< figure src="Imagens-Tutorial-Hugo/18-hugo-server-comando.jpeg" title="hugo server command in the terminal" alt="hugo server command in the terminal" >}}

Open your browser at `http://localhost:1313`. You'll see the blog template ready. The visual is pretty simple, but don't worry — you can change that later.

{{< figure src="Imagens-Tutorial-Hugo/19-modelo-blog-pronto.jpeg" title="Blog template ready" alt="Blog template ready" >}}

### Step 7: The Free Deploy (GitHub Actions)

Let's get this live for free.

Before we push the code, we need to tell Git not to save temporary files or the final site folder (since the GitHub robot will handle that for us). Create a file called `.gitignore` in the root of your project and paste the following:
```plaintext
public/
resources/
.hugo_build.lock
```

Just so you understand each line:

`public/`: This is the folder where Hugo dumps the final static HTML files when you run the build command. Since we use GitHub Actions to compile directly on their servers, we don't need to push the folder generated on our own machine.

`resources/`: Hugo creates this folder as a temporary cache to process images and styles faster on subsequent runs. It doesn't need to be versioned.

`.hugo_build.lock`: A small safety lock file created by Hugo to prevent two instances from running the build simultaneously. It serves no purpose in the repository.

Create an empty repository on GitHub by clicking the green button.

{{< figure src="Imagens-Tutorial-Hugo/19.1-new-repository.png" title="New button" alt="New button to create a new repository on GitHub" >}}

Don't forget to configure whether you want your repository to be public or private. If it's public, anyone can access the files you push to that GitHub repository. If it's private, only you or those you grant permission can see them.

{{< figure src="Imagens-Tutorial-Hugo/20-github-repositorio.jpeg" title="Repository being created on GitHub" alt="Repository being created on GitHub" >}}

In your terminal (still inside the meu-blog folder), initialize Git:

```bash
git init
git add .
git commit -m "initial commit"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
git push -u origin main
```

{{< figure src="Imagens-Tutorial-Hugo/21-git-init.jpeg" title="Commands to initialize Git" alt="Commands to initialize Git" >}}

{{< figure src="Imagens-Tutorial-Hugo/22-commit-inicial.jpeg" title="Commands for the first code save to GitHub" alt="Commands for the first code save to GitHub" >}}

{{< figure src="Imagens-Tutorial-Hugo/23-git-remote.jpeg" title="Command to rename your branch (convention) and connect your terminal to the GitHub repository" alt="Command to rename your branch (convention) and connect your terminal to the GitHub repository" >}}

{{< figure src="Imagens-Tutorial-Hugo/24-primeiro-push.jpeg" title="Command to push your code to GitHub for the first time" alt="Command to push your code to GitHub for the first time" >}}


On GitHub, go to the **Settings > Pages** tab. Under **Source**, choose **GitHub Actions**.
{{< figure src="Imagens-Tutorial-Hugo/25-github-settings.jpeg" title="Accessing Settings on GitHub from your project repository" alt="Indication of where to access Settings on GitHub from your project repository" >}}

{{< figure src="Imagens-Tutorial-Hugo/26-source-github-pages.jpeg" title="Setting Source to use GitHub Actions" alt="Indication of where to set Source to use GitHub Actions" >}}

Go back to your code editor. Inside your project folder (meu-blog), create a folder called `.github`. Inside it, create another called `workflows`, and inside that create a file called `hugo.yaml` (the exact final path must be `meu-blog/.github/workflows/hugo.yaml`).

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

{{< figure src="Imagens-Tutorial-Hugo/27-workflows.jpeg" title="Folder structure: workflows folder inside .github folder with hugo.yaml file" alt="Folder structure: workflows folder inside .github folder with hugo.yaml file" >}}

Run `git add .` in the terminal, then `commit` and `push`. The robot will "wake up", compile your site, and make it public at your official address.
{{< figure src="Imagens-Tutorial-Hugo/28-segundo-push.jpeg" title="Commands to push changes to the GitHub repository" alt="Commands to push changes to the GitHub repository" >}}

If you go to the **Actions** tab on GitHub, you can watch the build process running (shown in yellow) or completed (shown in green).

{{< figure src="Imagens-Tutorial-Hugo/30-GitHubActions-building.jpeg" title="GitHub Actions with build in progress" alt="GitHub Actions with build in progress" >}}

{{< figure src="Imagens-Tutorial-Hugo/31-GitHubActions-Estado-Final.png" title="GitHub Actions with build completed" alt="GitHub Actions with build completed" >}}

Every time you do a new *git push* to send changes to GitHub (for example, when you finish a new markdown file — a new post), GitHub Actions takes care of publishing the most up-to-date version of your blog. Just don't forget to use *git add* to stage your files first, and *git commit* to save the changes before pushing to GitHub (otherwise you won't actually send anything).

After completing this tutorial, you should have a simple initial structure. I suggest asking an AI to help you make it look nicer and adapt it however you want. The important part — the structure — is already done.


## Useful Resources

You can check out the [Hugo Official Documentation](https://gohugo.io/) to learn more. You'll find a lot of important information by clicking the "Docs" tab.

A video that helped me a lot when starting to learn Hugo was this one by **Fireship**:
{{< youtube 0RKpf3rK57I >}}


I also recommend the video by **NetworkChuck**, where he teaches you how to create a Hugo blog, apply a theme, and use **Obsidian** to write posts.
{{< youtube dnE7c0ELEH8 >}}


## Closing Thoughts

As I already mentioned in the first post I wrote for this blog, one of my goals is to progressively improve my writing. I know this article is nowhere near the level I want it to reach, but I think it's a decent introduction for anyone looking to learn about Hugo. I encourage you to read the official documentation, as that's where you'll find everything you need. If you don't know English, you can use tools like Google Translate, or better yet, ask an AI to give you explanations from the documentation translated into your native language.

I still know very little about Hugo and I'm learning more as I write these posts. If I feel like it, I might bring more posts talking about features and cool things we can do with this tool — like the Table of Contents logic (that heading navigation menu on the side) that I implemented on my own blog while writing this very post.