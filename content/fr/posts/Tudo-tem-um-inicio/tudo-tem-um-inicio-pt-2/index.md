---
title: "Tout a un début — Partie 2"
slug: "tout-a-un-debut-partie-2"
date: 2026-06-09T10:12:00-03:00
draft: false
categories: ["Début"]
series: ["Tout a un début"]
description: "La deuxième partie de la construction de MitzIdeas, axée sur l'installation et la configuration de Hugo."
translationKey: "primeiro-post-parte-2"
---

## Introduction

Dans le post précédent, j'ai partagé l'idée à l'origine de la naissance de ce blog. Aujourd'hui, nous allons ouvrir le capot de la machine et parler de l'ingénierie qui fait fonctionner MitzIdeas en ligne sans me coûter un seul centime d'hébergement, tout en maintenant une vitesse impressionnante et un contrôle total sur chaque pixel.

Je vais tenter d'expliquer davantage toute la structure qui maintient ce blog debout, de mieux détailler chaque concept, et à la fin proposer un mini tutoriel pour vous aider à partir de zéro vers votre premier blog créé avec Hugo.

---

## Le Moteur : Comprendre Hugo

Hugo est un **Générateur de Sites Statiques (Static Site Generator, SSG)** écrit dans le langage de programmation **Go**. Il a été créé par **Steve Francia** (connu sous le pseudonyme *spf13* dans l'écosystème Golang) en 2013, qui a conçu la structure de base de la façon dont le projet traite les templates et la fameuse logique de *lookup order*.

Vers 2015, la direction a été confiée à **Bjorn Erik Pedersen** (*bep*), qui a transformé ce moteur en une véritable voiture de course professionnelle. Parmi les réalisations les plus notables de Bjorn, on peut citer :

* **Performance Extrême :** Optimisation du compilateur pour tirer le meilleur parti des processeurs multi-cœurs, permettant aux sites de dizaines de milliers de pages d'être compilés en quelques secondes.
* **Hugo Pipes :** Traitement natif des assets, minification, fingerprinting et support robuste du multilinguisme (`i18n`).
* **Communauté :** Un dévouement presque surhumain à répondre aux *issues* et à réviser les *pull requests* sur GitHub, maintenant le projet à l'avant-garde du Web.

Grâce à ce framework flexible, Hugo est largement utilisé pour créer des portails de documentation, des sites gouvernementaux, corporatifs et, étonnamment, des blogs personnels. Il fonctionne comme un excellent **Content Management System (CMS)** de fichiers, où votre éditeur de code (comme VS Code) devient votre panneau de contrôle.

---

## Pourquoi utiliser Hugo plutôt que du HTML, CSS et JavaScript purs ?

Si vous créez un site uniquement avec du HTML, CSS et JS « pur » (le célèbre *Vanilla*), vous construisez une maison brique par brique. Cela fonctionne parfaitement pour 3 ou 5 pages. Mais que se passe-t-il quand vous avez 100 posts ?

Imaginez que j'aie 50 posts et que je décide de changer le lien de la page « À propos » dans l'en-tête :
* **En HTML pur :** Je devrais ouvrir **51 fichiers** (les 50 posts + la page `index.html`) et les modifier manuellement un par un. Si j'en oublie un, j'ai créé une incohérence.
* **Avec Hugo :** Je modifie un seul fichier partiel (`layouts/_partials/header.html`) et, en lançant la compilation, Hugo injecte ce changement dans toutes les pages instantanément. Hugo est une **fabrique de pages**.

De plus, écrire en Markdown (`.md`) est infiniment plus productif que d'assembler des balises HTML comme des pièces de LEGO. Vous vous concentrez sur le texte, et le moteur s'occupe du reste.

L'écosystème apporte également des avantages tels que la **Minification** (suppression des espaces blancs du code) et le **Fingerprinting** (technique de *cache busting* qui ajoute un *hash* unique au nom du fichier, comme `custom.min.8723.css`, forçant le navigateur de l'utilisateur à télécharger la version la plus récente et à ignorer le cache ancien).

### Quand NE PAS utiliser Hugo ?
Je ne suis pas là pour vendre Hugo comme une solution miracle. Il est excessif pour des *Landing Pages* simples d'une seule page. Il n'est pas non plus idéal pour les systèmes nécessitant des données traitées en temps réel côté serveur, comme des tableaux de bord avec connexion utilisateur ou des fils d'actualité dynamiques de réseaux sociaux. Dans ces scénarios, des frameworks comme Django (que j'utilise au quotidien) restent le bon choix.

---

## Ressources Fondamentales d'Architecture

### 1. Serveur de Développement Intégré
Quand vous tapez `hugo server` dans le terminal, il lance un serveur local qui crée un « tunnel » entre vos fichiers et le navigateur. Chaque fois que vous sauvegardez un fichier, il détecte les changements et recharge la page en quelques millisecondes.

Comme Hugo est fait en Go, nous avons accès aux **Go Templates** pour interpoler et transformer des données dynamiques. Ceux qui viennent de l'écosystème Python/Django remarqueront une similitude immédiate. C'est un environnement entièrement *batteries included*.

### 2. Hiérarchie de Layouts
Hugo travaille avec des moules. Vous définissez un fichier principal appelé `baseof.html` avec le squelette du site (ce qui se répète partout, comme le header et le footer) et vous ouvrez un « trou » en utilisant la commande `{{ block "main" . }}{{ end }}`. Les autres templates héritent simplement de ce moule et remplissent le trou.

### 3. Taxonomies Natives
Classer le contenu dans Hugo est extrêmement puissant et ne nécessite aucun plugin. Le système de **taxonomies** crée des relations logiques complexes (comme les tags, catégories et séries) en générant un index inversé et pondéré. Il sait exactement quels posts sont liés entre eux sans avoir besoin d'effectuer une seule requête SQL.

### 4. Dictionnaire de Fichiers Étranges
* `archetypes/` : Stocke vos templates de brouillon. Le fichier `default.md` définit quels champs seront pré-remplis par défaut chaque fois que vous créez un nouveau post.
* `.toml` : Le format de fichier de configuration (comme `hugo.toml`). C'est le cerveau global du site.
* `single.html` vs `list.html` : Le premier dicte le visuel d'un post unique ; le second rend les pages qui listent plusieurs posts (comme les index de catégories).

---

## Le Modèle de Sécurité : « Confiance Restreinte »

Hugo fonctionne selon un principe très clair : **il fait confiance à celui qui écrit le code (le développeur), mais pas à celui qui écrit le contenu (le Markdown).** Sa politique native fonctionne sur la base de listes de permissions (*allowlists*). Par défaut, les accès aux commandes du système d'exploitation (`os/exec`), les communications distantes et l'utilisation de formats de contenu comme le HTML pur (`allowContent`) sont totalement bloqués pour éviter les attaques par injection de scripts malveillants (XSS). Il n'exécute que les outils externes que vous autorisez explicitement, comme `tailwindcss` ou `postcss`.

Et comme le résultat final publié n'est constitué que de fichiers statiques sauvegardés sur disque, **99 % des vulnérabilités traditionnelles des serveurs (comme l'injection SQL) cessent d'exister.**

---

## Changements de la version 0.146.0 qui m'ont donné du fil à retordre

L'équipe de développement de Hugo a complètement refait le système de traitement des templates dans la version `v0.146.0`. Cela a cassé la compatibilité avec presque tous les tutoriels plus anciens disponibles sur Internet. Si vous lisez un tutoriel qui vous demande de créer le dossier `_default` ou le fichier `index.html`, sachez qu'il est obsolète.

Voici ce qui a changé et que vous devez appliquer selon les standards modernes :
* **Fin du dossier `_default` :** Les layouts par défaut se trouvent désormais directement à la racine du dossier `layouts/`.
* **Dossiers système avec `_` :** Les dossiers internes du moteur nécessitent désormais un *underscore* devant. Ainsi, `partials/` est devenu `_partials/` et `shortcodes/` est devenu `_shortcodes/`.
* **Fin de `index.html` pour la Home :** Le fichier responsable du rendu de la page d'accueil doit obligatoirement s'appeler `home.html`.
* **Nomenclature de Base :** Les identifiants des templates de base ont remplacé le tiret par un point. L'ancien `list-baseof.html` doit maintenant être renommé en `baseof.list.html`.

Vous pouvez consulter (et je le recommande vivement) les modifications apportées aux templates dans la [Documentation Officielle](https://gohugo.io/templates/new-templatesystem-overview/).

---

## RSS dans Hugo : Autonomie pour le Lecteur

Le **RSS (Really Simple Syndication)** est le « Twitter de l'ancien temps » qui survit encore comme un outil fantastique. Il génère un fichier au format `XML` contenant un résumé structuré de vos nouveautés (titre, lien, date).

Grâce à lui, le lecteur ne dépend pas de l'algorithme d'une grande corporation technologique pour voir votre contenu ; il lui suffit de s'abonner à votre feed dans des lecteurs comme Feedly ou Inoreader pour être notifié en temps réel. De plus, vous pouvez connecter des outils comme Zapier ou IFTTT à votre RSS pour automatiser des publications sur LinkedIn ou des notifications sur Discord chaque fois qu'un post est publié. Hugo génère ce fichier (`index.xml`) de façon totalement native.

---

## Globaliser le Blog avec le Mode Multilingue

Hugo gère plusieurs langues de façon spectaculaire grâce à des tables de traduction. Il existe deux méthodes pour gérer vos fichiers : par **Suffixes** (ex : `post.en.md`, `post.pt-br.md`) ou par **Répertoires**.

### Suffixes vs. Répertoires : Apprenez de mon erreur
Au départ, j'ai adopté la méthode des suffixes car elle semblait plus simple. Mais au moment où j'ai décidé d'organiser mes posts en couches et séries profondes (comme `content/posts/Livros/Filosofia/`), mélanger les fichiers de cinq langues différentes dans le même dossier a transformé le projet en un chaos absolu.

**La Solution :** J'ai migré vers la traduction par **Répertoires**. Dans `hugo.toml`, j'ai configuré des dossiers isolés pour chaque langue (`content/pt-br/`, `content/en/`, etc.). Maintenant, les fichiers Markdown de toutes les langues s'appellent simplement `index.md`, maintenant l'arborescence de répertoires parfaitement propre.

Pour relier les pages, on utilise l'attribut `translationKey` dans l'en-tête du post. Peu importe si le dossier en portugais s'appelle `Tudo-tem-um-inicio` et celui en français `Tout-a-un-debut` ; si les deux ont la même `translationKey`, Hugo crée automatiquement le lien dans le sélecteur de langue du menu.

Pour séparer les chaînes d'interface (comme les boutons « Retour » ou « Rechercher »), on utilise le dossier `i18n/` avec des fichiers `.toml` pour chaque langue, en les appelant dans le HTML via la fonction `{{ T "cle_de_la_chaine" }}`.

Sur cette page de la [Documentation Officielle de Hugo](https://gohugo.io/content-management/multilingual/) vous pouvez en savoir plus sur la gestion de différentes langues.

---

## URLs Propres : Le Secret des Slugs et Permalinks

Par défaut, Hugo reflète le chemin physique de vos dossiers directement dans l'URL du site. Cela génère des liens longs et redondants. Pire encore : si vous gardez le nom de vos dossiers en portugais pour votre propre organisation mentale, votre URL en anglais hériterait du terme en portugais.

Pour contourner cela, on utilise deux propriétés :
1.  **Slugs :** Dans le *Front Matter* de chaque fichier, on définit le paramètre `slug = "nom-propre-de-url"`. Il force l'URL de ce post à adopter un terme spécifique entièrement en minuscules.
2.  **Permalinks :** Dans le fichier `hugo.toml`, on définit une règle globale pour centraliser les routes, nettoyant les chemins intermédiaires des dossiers :

```toml
[permalinks]
  posts = "/posts/:slug/"
```

Cela garantit que, peu importe dans quel sous-dossier le Markdown est caché, l'URL finale générée pour l'utilisateur sera courte, élégante et favorable au SEO.

### GitHub Pages et GitHub Actions
Pour mettre le blog en ligne, j'ai combiné deux outils gratuits de GitHub :

**GitHub Pages** : Fonctionne comme notre hébergement statique. Il ne traite pas les langages serveur ; il agit simplement comme une vitrine qui affiche les fichiers HTML, CSS et JS purs résultant de la compilation.

**GitHub Actions** : Comme notre dépôt ne contient que les fichiers de développement (Markdowns et layouts), nous avons besoin de quelqu'un pour compiler le site. Actions est un ordinateur virtuel (un robot tournant sous Linux Ubuntu) qui est en attente. Chaque fois que je fais un *git push main*, ce robot « se réveille », lit notre fichier d'instructions `hugo.yaml`, installe la version correcte de **Hugo Extended**, compile tout le projet et livre le dossier `public/` prêt pour que GitHub Pages l'affiche au monde entier.

## Mini Tutoriel : Créer votre propre blog avec Hugo
Passons aux choses sérieuses et créons un blog structuré de zéro, en appliquant toutes les règles modernes de la version 0.146.0+ (autant commencer votre blog avec la version la plus récente).
Je vous recommande de suivre le tutoriel avec les commandes exactes que j'ai fournies, juste pour voir en pratique comment un blog est créé de zéro avec Hugo. Après cela, vous pouvez le refaire en adaptant les noms et tout le reste comme vous le souhaitez.

### Étape 1 : Préparer le Terrain (Les Outils)
Pour que notre « fabrique » de pages fonctionne, nous avons besoin de trois outils :

**Git :** Le système de gestion de versions de code.

**Go :** Le langage de base de notre moteur.

**Hugo (Version Extended) :** Notre générateur de sites statiques.



#### Option A : Installation via Terminal (Recommandé)
Sous Windows (PowerShell en Administrateur) :

PowerShell
```powershell
winget install Git.Git 
winget install GoLang.Go 
winget install Hugo.Hugo.Extended
```

Sous Linux (Ubuntu/Debian) :

```bash
sudo apt update 
sudo apt install git golang hugo
```

Sous macOS (Homebrew) :
```bash
brew install git go hugo
```

#### Option B : Installation Manuelle (Interface Graphique)
Téléchargez et installez Git et Go en cliquant sur **« Suivant »** dans les installateurs standard.

Pour Hugo, rendez-vous dans les Releases du dépôt GitHub officiel du projet et téléchargez la version `.zip` correspondant à votre système d'exploitation (assurez-vous de télécharger la version avec *extended* dans le nom).

Extrayez les fichiers dans un dossier sécurisé (ex : `C:\Hugo\`).

Sous Windows : Cherchez dans le menu démarrer « Modifier les variables d'environnement système ». Cliquez sur « Variables d'environnement », double-cliquez sur Path et ajoutez le chemin du dossier où vous avez placé Hugo.

Fermez le terminal, rouvrez-le et tapez `hugo version`. Si la version s'affiche, nous sommes prêts.

### Étape 2 : Les Fondations du Projet

{{< figure src="Imagens-Tutorial-Hugo/01-VS-Code-Inicial.jpeg" title="Vue initiale de VS Code (éditeur de code) avec le dossier du projet ouvert" alt="Vue initiale de VS Code (éditeur de code) avec le dossier du projet ouvert." >}}

Ouvrez votre terminal dans le dossier où vous souhaitez stocker le projet et tapez :

``` bash
hugo new site mon-blog
cd mon-blog
```

{{< figure src="Imagens-Tutorial-Hugo/02-Criando-o-site.jpeg" title="Vue initiale des premières commandes dans le terminal VS Code qui initialisent le projet de blog créé avec Hugo" alt="Vue initiale des premières commandes dans le terminal VS Code qui initialisent le projet de blog créé avec Hugo." >}}

**Attention :** La commande `cd mon-blog` est cruciale. Elle garantit que votre terminal se trouve à l'intérieur du dossier du projet. Toutes les prochaines étapes doivent être exécutées à partir d'ici.

### Étape 3 : Configuration de la Base et du Multilinguisme
Préparons le projet pour prendre en charge plusieurs langues en utilisant la méthodologie par Répertoires.

Ouvrez le fichier `hugo.toml` et remplacez tout par ceci (n'oubliez pas de remplacer la baseURL par votre nom d'utilisateur GitHub) :

```toml
baseURL = "https://VOTRE-UTILISATEUR.github.io/mon-blog/"
title = "Mon Blog Professionnel"
defaultContentLanguage = "fr"

[permalinks]
  posts = "/posts/:slug/"

[languages]
  [languages."fr"]
    languageName = "Français"
    weight = 1
    contentDir = "content/fr"
```
{{< figure src="Imagens-Tutorial-Hugo/04-Hugo-TOML.jpeg" title="Fichier hugo.toml adapté" alt="Fichier hugo.toml adapté." >}}


Dans le terminal, créez les dossiers de base pour le contenu en français :

```bash
mkdir -p content/fr/posts
mkdir -p content/fr/about
```

{{< figure src="Imagens-Tutorial-Hugo/05-Pastas-Base-PTBR.jpeg" title="Commandes pour créer les dossiers de base pour le contenu en français" alt="Commandes pour créer les dossiers de base pour le contenu en français." >}}
{{< figure src="Imagens-Tutorial-Hugo/06-Estruturas-Pastas-Base-PTBR.jpeg" title="Votre structure de dossiers devrait ressembler à ceci" alt="Image de la structure de dossiers après les dernières commandes." >}}


### Étape 4 : Construire le Layout (Le Moule)
Créons les structures HTML qui vont recevoir nos textes, en intégrant la logique du Mode Sombre et de la Table des Matières latérale. Créez les fichiers ci-dessous dans le dossier `layouts/` :

**1. Le Squelette (layouts/baseof.html) :**

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
                <a href="{{ "/about/" | relLangURL }}">À propos</a>
                <button onclick="toggleTheme()" class="theme-btn" aria-label="Changer de thème">🌓</button>
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

{{< figure src="Imagens-Tutorial-Hugo/07-baseof-html.jpeg" title="Votre structure de dossiers devrait ressembler à ceci" alt="Image de la structure de dossiers après les dernières commandes." >}}


**2. La Page d'Accueil (layouts/home.html) :**

```html
{{ define "main" }}
<div class="container hero">
    <h2>Bienvenue !</h2>
    <p>Un espace minimaliste pour documenter des idées et des apprentissages.</p>
</div>
{{ end }}
```
{{< figure src="Imagens-Tutorial-Hugo/08-home-html.jpeg" title="Contenu du fichier home.html et structure de dossiers" alt="Contenu du fichier home.html et structure de dossiers." >}}


**3. Le Post Unique et la Page À propos (layouts/single.html) :**
Ici nous insérons le TableOfContents pour générer l'index dynamique.

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
            <span class="toc-title">Dans ce Post</span>
            {{ .TableOfContents }}
        </div>
    </aside>
</div>
{{ end }}
```
{{< figure src="Imagens-Tutorial-Hugo/09-single-html.jpeg" title="Contenu du fichier single.html et structure de dossiers" alt="Contenu du fichier single.html et structure de dossiers." >}}

**4. La Liste de Posts (layouts/list.html) :**

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
{{< figure src="Imagens-Tutorial-Hugo/10-list-html.jpeg" title="Contenu du fichier list.html et structure de dossiers" alt="Contenu du fichier list.html et structure de dossiers." >}}

### Étape 5 : La Peinture (Le CSS Professionnel)
Hugo a automatiquement généré un dossier `static/` à la racine du projet. Entrez dedans, créez un sous-dossier `css/` et un fichier appelé `custom.css`.

Ouvrez `static/css/custom.css` et collez ce code. Il possède des variables automatiques pour le Mode Sombre et stylise la barre latérale.

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
{{< figure src="Imagens-Tutorial-Hugo/11-css-custom.jpeg" title="Contenu du fichier custom.css et structure de dossiers" alt="Contenu du fichier custom.css et structure de dossiers." >}}

### Étape 6 : Créer le Contenu
**1. La Page « À propos »** :
Dans le terminal, créez la page À propos :

```bash
hugo new content content/fr/about/index.md
```
{{< figure src="Imagens-Tutorial-Hugo/12-about-page.jpeg" title="Commandes pour créer la page À propos" alt="Commandes pour créer la page À propos" >}}


Éditez le fichier nouvellement créé dans `content/fr/about/index.md` :

```markdown
---
title: "À propos de moi"
slug: "about"
isAbout: true
translationKey: "about"
---

Bonjour ! Je suis un développeur qui partage son parcours, ses études et ses idées sur ce blog.
```

(Note : on utilise `isAbout: true` pour que notre layout HTML sache qu'il ne doit pas afficher la date sur cette page).

{{< figure src="Imagens-Tutorial-Hugo/13-index-about.jpeg" title="État initial de l'index de la page À propos" alt="État initial de l'index de la page À propos" >}}

{{< figure src="Imagens-Tutorial-Hugo/14-pagina-sobre-editada.jpeg" title="Page À propos après modifications" alt="Page À propos après modifications" >}}

**2. Le Premier Post :**

```bash
hugo new content content/fr/posts/mon-premier-article/index.md
```
{{< figure src="Imagens-Tutorial-Hugo/15-my-first-article.jpeg" title="Commande pour créer le fichier du premier article" alt="Commande pour créer le fichier du premier article" >}}

{{< figure src="Imagens-Tutorial-Hugo/16-estado-inicial-primeiro-artigo-md.jpeg" title="État initial du markdown du premier article" alt="État initial du markdown du premier article" >}}

Éditez le fichier dans `content/fr/posts/mon-premier-article/index.md` :

```markdown
---
title: "Mon Premier Article"
slug: "mon-premier-article"
date: 2026-06-09T10:00:00-03:00
draft: false
translationKey: "primeiro-artigo"
---

Ci-dessous, nous explorons les avantages de la génération de sites statiques.

## Vitesse Absolue
Hugo compile les pages en quelques millisecondes.

## Sécurité
Comme il n'y a pas de base de données tournant en temps réel, les risques d'intrusion chutent drastiquement.
```

{{< figure src="Imagens-Tutorial-Hugo/17-primeiro-artigo-pos-alteracoes.jpeg" title="État du markdown du premier article après modifications" alt="État du markdown du premier article après modifications" >}}


Dans le terminal, lancez le serveur :

```bash
hugo server
```
{{< figure src="Imagens-Tutorial-Hugo/18-hugo-server-comando.jpeg" title="Commande hugo server dans le terminal" alt="Commande hugo server dans le terminal" >}}

Ouvrez le navigateur sur `http://localhost:1313`. Vous verrez le modèle de blog prêt. Le visuel est assez simple, mais ne vous inquiétez pas, vous pourrez le modifier ensuite.

{{< figure src="Imagens-Tutorial-Hugo/19-modelo-blog-pronto.jpeg" title="Modèle de blog prêt" alt="Modèle de blog prêt" >}}

### Étape 7 : Le Deploy Gratuit (GitHub Actions)

Mettons tout cela en ligne gratuitement.

Avant d'envoyer le code, nous devons indiquer à Git de ne pas sauvegarder les fichiers temporaires ni le dossier du site final (puisque le robot de GitHub s'en chargera pour nous). Créez un fichier appelé `.gitignore` à la racine de votre projet et collez ce qui suit :
```plaintext
public/
resources/
.hugo_build.lock
```

Pour bien comprendre chaque ligne :

`public/` : C'est le dossier où Hugo place les fichiers HTML statiques finaux quand vous lancez la commande de compilation. Comme nous utilisons GitHub Actions pour compiler directement sur leurs serveurs, nous n'avons pas besoin d'envoyer le dossier généré sur notre ordinateur.

`resources/` : Hugo crée ce dossier comme cache temporaire pour traiter les images et les styles plus rapidement les prochaines fois. Il n'a pas besoin d'être versionné.

`.hugo_build.lock` : C'est un petit fichier de verrouillage de sécurité créé par Hugo pour empêcher deux instances de lancer le build simultanément. Il ne sert à rien dans le dépôt.

Créez un dépôt vide sur GitHub en cliquant sur le bouton vert.

{{< figure src="Imagens-Tutorial-Hugo/19.1-new-repository.png" title="Bouton New" alt="Bouton New pour créer un nouveau dépôt sur GitHub" >}}

N'oubliez pas de configurer si vous souhaitez que votre dépôt soit public ou privé. S'il est public, n'importe qui peut accéder aux fichiers que vous envoyez sur GitHub. S'il est privé, seul vous ou ceux qui ont la permission peuvent les voir.

{{< figure src="Imagens-Tutorial-Hugo/20-github-repositorio.jpeg" title="Dépôt en cours de création sur GitHub" alt="Dépôt en cours de création sur GitHub" >}}

Dans votre terminal (toujours dans le dossier mon-blog), initialisez Git :

```bash
git init
git add .
git commit -m "commit initial"
git branch -M main
git remote add origin https://github.com/VOTRE-UTILISATEUR/VOTRE-DEPOT.git
git push -u origin main
```

{{< figure src="Imagens-Tutorial-Hugo/21-git-init.jpeg" title="Commandes pour initialiser Git" alt="Commandes pour initialiser Git" >}}

{{< figure src="Imagens-Tutorial-Hugo/22-commit-inicial.jpeg" title="Commandes pour la première sauvegarde du code sur GitHub" alt="Commandes pour la première sauvegarde du code sur GitHub" >}}

{{< figure src="Imagens-Tutorial-Hugo/23-git-remote.jpeg" title="Commande pour renommer votre branche (convention) et connecter votre terminal au dépôt sur GitHub" alt="Commande pour renommer votre branche (convention) et connecter votre terminal au dépôt sur GitHub" >}}

{{< figure src="Imagens-Tutorial-Hugo/24-primeiro-push.jpeg" title="Commande pour envoyer votre code sur GitHub pour la première fois" alt="Commande pour envoyer votre code sur GitHub pour la première fois" >}}


Sur GitHub, allez dans l'onglet **Settings > Pages**. Dans **Source**, choisissez **GitHub Actions**.
{{< figure src="Imagens-Tutorial-Hugo/25-github-settings.jpeg" title="Accéder à Settings ou Configuration sur GitHub depuis le dépôt de votre projet" alt="Indication de l'emplacement pour accéder à Settings ou Configuration sur GitHub depuis le dépôt de votre projet" >}}

{{< figure src="Imagens-Tutorial-Hugo/26-source-github-pages.jpeg" title="Définir Source pour utiliser GitHub Actions" alt="Indication de l'emplacement pour définir Source afin d'utiliser GitHub Actions" >}}

Revenez dans votre éditeur de code. À l'intérieur du dossier de votre projet (mon-blog), créez un dossier appelé `.github`. À l'intérieur, créez un autre dossier appelé `workflows`, et là créez un fichier appelé `hugo.yaml` (le chemin final exact doit être `mon-blog/.github/workflows/hugo.yaml`).

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

{{< figure src="Imagens-Tutorial-Hugo/27-workflows.jpeg" title="Structure de dossiers : dossier workflows dans le dossier .github avec le fichier hugo.yaml" alt="Structure de dossiers : dossier workflows dans le dossier .github avec le fichier hugo.yaml" >}}

Faites un `git add .` dans le terminal, puis le `commit` et le `push`. Le robot va « se réveiller », compiler votre site et le rendre public à votre adresse officielle.
{{< figure src="Imagens-Tutorial-Hugo/28-segundo-push.jpeg" title="Commandes pour envoyer les modifications au dépôt GitHub" alt="Commandes pour envoyer les modifications au dépôt GitHub" >}}

Si vous allez dans l'onglet **Actions** de GitHub, vous pouvez voir le processus de *build* en cours d'exécution (en jaune) ou terminé (en vert).

{{< figure src="Imagens-Tutorial-Hugo/30-GitHubActions-building.jpeg" title="GitHub Actions avec build en cours" alt="GitHub Actions avec build en cours" >}}

{{< figure src="Imagens-Tutorial-Hugo/31-GitHubActions-Estado-Final.png" title="GitHub Actions avec build terminé" alt="GitHub Actions avec le build terminé" >}}

Chaque fois que vous faites un nouveau *git push* pour envoyer les modifications sur GitHub (par exemple, quand vous finalisez un nouveau fichier markdown, c'est-à-dire un post), GitHub Actions se charge de mettre votre blog en ligne dans sa version la plus récente. N'oubliez pas d'utiliser *git add* pour ajouter les fichiers au Staging et *git commit* pour sauvegarder les modifications avant d'envoyer sur GitHub (sinon vous n'enverrez rien).

Après avoir terminé ce tutoriel, vous devriez avoir une structure initiale assez simple. Je vous suggère de demander à une IA de vous aider à l'embellir et à l'adapter selon vos souhaits. L'important, c'est-à-dire la structure, a déjà été fait.


## Ressources utiles

Vous pouvez consulter la [Documentation Officielle de Hugo](https://gohugo.io/) pour en apprendre davantage. Vous trouverez de nombreuses informations importantes en accédant à l'onglet « Docs ».

Une vidéo qui m'a beaucoup aidé à commencer à apprendre Hugo est celle de **Fireship** (contenu en anglais) :
{{< youtube 0RKpf3rK57I >}}


Je recommande également la vidéo de **NetworkChuck** (contenu en anglais), où il enseigne comment créer un blog avec Hugo, appliquer un thème et utiliser **Obsidian** pour écrire les posts.
{{< youtube dnE7c0ELEH8 >}}


## Conclusion

Comme je l'ai déjà mentionné dans le premier post de ce blog, l'un de mes objectifs est d'améliorer progressivement mon écriture. Je sais que cet article est loin du niveau que je souhaite atteindre, mais je pense que c'est déjà une introduction correcte pour ceux qui cherchent à apprendre Hugo. Je vous encourage à lire la documentation officielle, car c'est là que vous trouverez tout ce dont vous avez besoin. Si vous ne lisez pas l'anglais, vous pouvez utiliser des outils comme Google Translate, ou mieux encore, demander à une IA de vous fournir des explications de la documentation traduites dans votre langue maternelle.

Je connais encore très peu Hugo et j'apprends davantage au fur et à mesure que j'écris ces posts. Si l'envie m'en prend, j'apporterai peut-être d'autres posts sur les fonctionnalités et les choses intéressantes que l'on peut faire avec cet outil, comme par exemple la logique de la Table of Contents (ce menu de navigation par titres de page) que j'ai implémentée dans mon propre blog en écrivant ce post.