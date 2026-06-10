---
title: "Alles hat einen Anfang — Teil 2"
slug: "alles-hat-einen-anfang-teil-2"
date: 2026-06-09T10:12:00-03:00
draft: false
categories: ["Anfang"]
series: ["Alles hat einen Anfang"]
description: "Der zweite Teil der Entstehung von MitzIdeas mit Schwerpunkt auf der Installation und Konfiguration von Hugo."
translationKey: "primeiro-post-parte-2"
---

## Einleitung

Im vorherigen Beitrag habe ich die Idee hinter der Entstehung dieses Blogs vorgestellt. Heute öffnen wir die Motorhaube und werfen einen Blick auf die Technik, die MitzIdeas am Laufen hält – ohne dass mich das auch nur einen einzigen Cent für Hosting kostet, bei gleichzeitig enormer Geschwindigkeit und vollständiger Kontrolle über jedes einzelne Pixel.

Ich werde etwas ausführlicher auf die gesamte Struktur eingehen, die diesen Blog trägt, die einzelnen Konzepte genauer erklären und am Ende ein kleines Tutorial anbieten, das dir dabei hilft, dein erstes mit Hugo erstelltes Blog von Grund auf aufzubauen.

---

## Der Motor: Hugo verstehen

Hugo ist ein **Static Site Generator (SSG)**, der in der Programmiersprache **Go** geschrieben wurde. Er wurde 2013 von **Steve Francia** (im Golang-Ökosystem als *spf13* bekannt) entwickelt, der die grundlegende Architektur dafür entwarf, wie das Projekt Templates verarbeitet und die bekannte *Lookup Order*-Logik umsetzt.

Etwa im Jahr 2015 ging die Leitung an **Bjorn Erik Pedersen** (*bep*) über, der diesen Motor in einen echten professionellen Rennwagen verwandelte. Zu Bjorns bemerkenswertesten Leistungen gehören:

* **Extreme Performance:** Optimierung des Compilers, um Multi-Core-Prozessoren maximal auszunutzen, sodass selbst Websites mit Zehntausenden von Seiten innerhalb weniger Sekunden kompiliert werden können.
* **Hugo Pipes:** Native Verarbeitung von Assets, Minifizierung, Fingerprinting und robuste Unterstützung für Mehrsprachigkeit (`i18n`).
* **Community:** Ein beinahe übermenschliches Engagement beim Beantworten von *Issues* und Überprüfen von *Pull Requests* auf GitHub, wodurch das Projekt kontinuierlich an der Spitze der Webentwicklung bleibt.

Dank dieses flexiblen Frameworks wird Hugo häufig für Dokumentationsportale, Regierungswebsites, Unternehmensseiten und – man glaubt es kaum – persönliche Blogs eingesetzt. Es fungiert als hervorragendes dateibasiertes **Content Management System (CMS)**, bei dem dein Code-Editor (zum Beispiel VS Code) zu deinem eigentlichen Administrationsbereich wird.

---

## Warum Hugo statt reinem HTML, CSS und JavaScript verwenden?

Wenn du eine Website ausschließlich mit reinem HTML, CSS und JavaScript (dem berühmten *Vanilla*) erstellst, baust du ein Haus Stein für Stein. Das funktioniert wunderbar bei 3 oder 5 Seiten. Aber was passiert, wenn du 100 Beiträge hast?

Stell dir vor, ich hätte 50 Beiträge und würde den Link zur Seite „Über mich“ im Header ändern wollen:

* **Mit reinem HTML:** Ich müsste **51 Dateien** öffnen (die 50 Beiträge plus die `index.html`) und jede einzelne manuell bearbeiten. Wenn ich eine vergesse, entsteht eine Inkonsistenz.
* **Mit Hugo:** Ich ändere lediglich eine einzige Partial-Datei (`layouts/_partials/header.html`), und beim nächsten Build übernimmt Hugo diese Änderung automatisch auf allen Seiten. Hugo ist eine echte **Seitenfabrik**.

Außerdem ist das Schreiben in Markdown (`.md`) unendlich produktiver, als HTML-Tags wie LEGO-Steine zusammenzusetzen. Du konzentrierst dich auf den Inhalt, und die Engine kümmert sich um den Rest.

Das Ökosystem bietet darüber hinaus Vorteile wie **Minifizierung** (das Entfernen überflüssiger Leerzeichen aus dem Code) und **Fingerprinting** (eine *Cache Busting*-Technik, die dem Dateinamen einen eindeutigen Hash hinzufügt, zum Beispiel `custom.min.8723.css`, sodass der Browser automatisch die neueste Version lädt und den alten Cache ignoriert).

### Wann sollte man Hugo NICHT verwenden?

Ich bin nicht hier, um Hugo als Allheilmittel zu verkaufen. Für einfache einseitige *Landing Pages* ist es oft überdimensioniert. Ebenso eignet es sich nicht ideal für Systeme, die serverseitig in Echtzeit Daten verarbeiten müssen, wie Dashboards mit Benutzeranmeldung oder dynamische Social-Media-Feeds. In solchen Szenarien bleiben Frameworks wie Django (das ich täglich verwende) die richtige Wahl.

---

## Grundlegende Architekturkonzepte

### 1. Integrierter Entwicklungsserver

Wenn du im Terminal `hugo server` eingibst, startet Hugo einen lokalen Server, der eine direkte Verbindung zwischen deinen Dateien und dem Browser herstellt. Jedes Mal, wenn du eine Datei speicherst, erkennt Hugo die Änderungen und lädt die Seite innerhalb von Millisekunden neu.

Da Hugo in Go geschrieben wurde, haben wir Zugriff auf die **Go Templates**, um dynamische Daten zu interpolieren und zu transformieren. Wer aus dem Python-/Django-Ökosystem kommt, wird sofort gewisse Ähnlichkeiten erkennen. Es handelt sich um eine vollständig *batteries included*-Umgebung.

### 2. Layout-Hierarchie

Hugo arbeitet mit Vorlagen. Du definierst eine Hauptdatei namens `baseof.html`, die das Grundgerüst der Website enthält (also alles, was sich überall wiederholt, etwa Header und Footer), und öffnest mit `{{ block "main" . }}{{ end }}` eine Art „Lücke“. Die übrigen Templates erben diese Vorlage und füllen lediglich diesen Bereich aus.

### 3. Native Taxonomien

Inhalte in Hugo zu klassifizieren ist äußerst leistungsfähig und benötigt keinerlei Plugins. Das **Taxonomie-System** erzeugt komplexe logische Beziehungen (zum Beispiel Tags, Kategorien und Serien), indem es einen gewichteten invertierten Index erstellt. Dadurch weiß Hugo genau, welche Beiträge miteinander zusammenhängen, ohne auch nur eine einzige SQL-Abfrage ausführen zu müssen.

### 4. Wörterbuch der merkwürdigen Dateien

* `archetypes/`: Enthält deine Entwürfe und Vorlagen. Die Datei `default.md` definiert, welche Felder standardmäßig ausgefüllt werden, wenn du einen neuen Beitrag erstellst.
* `.toml`: Das Konfigurationsformat (zum Beispiel `hugo.toml`). Es bildet das zentrale Gehirn der gesamten Website.
* `single.html` vs. `list.html`: Ersteres definiert die Darstellung eines einzelnen Beitrags, Letzteres rendert Seiten mit mehreren Beiträgen (zum Beispiel Kategorieübersichten).

---

## Das Sicherheitsmodell: „Eingeschränktes Vertrauen“

Hugo folgt einem sehr klaren Prinzip: **Es vertraut demjenigen, der den Code schreibt (dem Entwickler), vertraut jedoch nicht demjenigen, der den Inhalt schreibt (dem Markdown).** Die integrierte Sicherheitsrichtlinie basiert auf *Allowlists*. Standardmäßig sind Zugriffe auf Betriebssystembefehle (`os/exec`), Remote-Kommunikation sowie Formate wie reines HTML (`allowContent`) vollständig blockiert, um Angriffe wie die Einschleusung bösartiger Skripte (XSS) zu verhindern. Externe Werkzeuge werden nur dann ausgeführt, wenn du sie ausdrücklich freigibst, etwa `tailwindcss` oder `postcss`.

Da das veröffentlichte Endergebnis ausschließlich aus statischen Dateien besteht, die auf der Festplatte gespeichert werden, **entfallen 99 % der klassischen Server-Schwachstellen (wie SQL Injection) vollständig.**

---

## Änderungen in Version 0.146.0, die mir Kopfzerbrechen bereitet haben

Das Hugo-Entwicklungsteam hat in Version `v0.146.0` das gesamte Template-Verarbeitungssystem grundlegend überarbeitet. Dadurch wurden nahezu alle älteren Tutorials im Internet inkompatibel. Wenn du auf ein Tutorial stößt, das dich anweist, einen `_default`-Ordner oder eine `index.html` anzulegen, solltest du wissen, dass es veraltet ist.

Folgende Änderungen musst du im modernen Standard berücksichtigen:

* **Wegfall des `_default`-Ordners:** Standardlayouts befinden sich jetzt direkt im Wurzelverzeichnis von `layouts/`.
* **Systemordner mit `_`:** Interne Ordner der Engine benötigen nun einen führenden Unterstrich. Aus `partials/` wurde also `_partials/` und aus `shortcodes/` wurde `_shortcodes/`.
* **Kein `index.html` mehr für die Startseite:** Die Datei, die die Homepage rendert, muss jetzt zwingend `home.html` heißen.
* **Neue Namenskonvention für Basis-Templates:** Die Bezeichner verwenden jetzt statt eines Bindestrichs einen Punkt. Das frühere `list-baseof.html` heißt nun `baseof.list.html`.

Die Änderungen an den Templates kannst du (und das empfehle ich ausdrücklich) in der offiziellen Dokumentation nachlesen:

https://gohugo.io/templates/new-templatesystem-overview/

---

## RSS in Hugo: Mehr Unabhängigkeit für den Leser

**RSS (Really Simple Syndication)** ist das „Twitter der alten Zeiten“, das bis heute als hervorragendes Werkzeug überlebt hat. Es erzeugt eine `XML`-Datei mit einer strukturierten Zusammenfassung deiner Neuigkeiten (Titel, Link und Datum).

Dadurch ist der Leser nicht auf den Algorithmus eines großen Technologieunternehmens angewiesen, um deine Inhalte zu sehen. Es genügt, deinen Feed in einem Reader wie Feedly oder Inoreader zu abonnieren, um sofort benachrichtigt zu werden. Darüber hinaus kannst du Werkzeuge wie Zapier oder IFTTT mit deinem RSS-Feed verbinden, um automatische LinkedIn-Beiträge zu veröffentlichen oder Discord-Benachrichtigungen zu versenden, sobald ein neuer Beitrag erscheint. Hugo erzeugt diese Datei (`index.xml`) vollständig nativ.

---

## Das Blog mit dem Multilingual Mode internationalisieren

Hugo verwaltet mehrere Sprachen auf beeindruckende Weise über Übersetzungstabellen. Es gibt zwei Möglichkeiten, deine Dateien zu organisieren: über **Suffixe** (z. B. `post.en.md`, `post.pt-br.md`) oder über **Verzeichnisse**.

### Suffixe vs. Verzeichnisse: Lerne aus meinem Fehler

Anfangs habe ich mich für die Suffix-Methode entschieden, weil sie einfacher wirkte. Doch als ich begann, meine Beiträge in tief verschachtelten Ebenen und Serien zu organisieren (zum Beispiel `content/posts/Livros/Filosofia/`), verwandelte das Vermischen von Dateien aus fünf verschiedenen Sprachen im selben Ordner das Projekt in ein absolutes Chaos.

**Die Lösung:** Ich bin auf die Übersetzung über **Verzeichnisse** umgestiegen. In der `hugo.toml` habe ich für jede Sprache eigene Ordner eingerichtet (`content/pt-br/`, `content/en/` usw.). Dadurch heißen die Markdown-Dateien in allen Sprachen einfach `index.md`, wodurch die Verzeichnisstruktur sauber und übersichtlich bleibt.

Um die Seiten miteinander zu verknüpfen, verwenden wir das Attribut `translationKey` im Header des Beitrags. Dabei spielt es keine Rolle, ob der portugiesische Ordner `Tudo-tem-um-inicio` und der französische `Tout-a-un-debut` heißt – solange beide denselben `translationKey` besitzen, erstellt Hugo die Verknüpfung im Sprachumschalter des Menüs automatisch.

Für die Oberflächen-Strings (wie die Schaltflächen „Zurück“ oder „Suchen“) verwenden wir den Ordner `i18n/` mit einer `.toml`-Datei für jede Sprache und binden sie im HTML über die Funktion `{{ T "chave_da_string" }}` ein.

Auf dieser Seite der offiziellen Hugo-Dokumentation kannst du mehr über die Verwaltung mehrerer Sprachen erfahren:

https://gohugo.io/content-management/multilingual/

---

## Saubere URLs: Das Geheimnis von Slugs und Permalinks

Standardmäßig übernimmt Hugo den physischen Pfad deiner Ordner direkt in die URL der Website. Das führt zu unnötig langen und redundanten Links. Noch problematischer wird es, wenn du deine Ordner aus organisatorischen Gründen auf Portugiesisch benennst – dann würde selbst deine englische URL portugiesische Begriffe enthalten.

Um dieses Problem zu lösen, verwenden wir zwei Eigenschaften:

1. **Slugs:** Im *Front Matter* jeder Datei definieren wir den Parameter `slug = "sauberer-url-name"`. Dadurch wird die URL dieses Beitrags auf einen bestimmten, vollständig kleingeschriebenen Begriff festgelegt.

2. **Permalinks:** In der Datei `hugo.toml` definieren wir eine globale Regel, die die Routen zentralisiert und überflüssige Zwischenordner aus dem URL-Pfad entfernt:

```toml
[permalinks]
  posts = "/posts/:slug/"
```

Dadurch wird sichergestellt, dass die endgültige URL für den Benutzer kurz, elegant und SEO-freundlich bleibt – unabhängig davon, in welchem Unterordner die Markdown-Datei tatsächlich gespeichert ist.

### GitHub Pages und GitHub Actions

Um das Blog kostenlos online zu stellen, habe ich zwei kostenlose GitHub-Dienste miteinander kombiniert:

**GitHub Pages:** Dient als statisches Hosting. Es verarbeitet keine serverseitigen Sprachen, sondern fungiert lediglich als Schaufenster für die kompilierten HTML-, CSS- und JavaScript-Dateien.

**GitHub Actions:** Da unser Repository nur die Entwicklungsdateien (Markdown-Dateien und Layouts) enthält, benötigen wir jemanden, der die Website kompiliert. GitHub Actions ist praktisch ein virtueller Computer (ein Ubuntu-Linux-Roboter), der ständig bereitsteht. Immer wenn ich `git push main` ausführe, „wacht“ dieser Roboter auf, liest unsere `hugo.yaml`, installiert die richtige Version von **Hugo Extended**, kompiliert das gesamte Projekt und stellt den fertigen `public/`-Ordner für GitHub Pages bereit.

## Mini-Tutorial: Dein eigenes Blog mit Hugo erstellen

Jetzt wird es praktisch: Wir erstellen ein strukturiertes Blog von Grund auf und wenden dabei alle modernen Standards der Version 0.146.0+ an (es gibt schließlich keinen Grund, nicht direkt mit der aktuellsten Version zu beginnen).

Ich empfehle dir, das Tutorial zunächst exakt mit den angegebenen Befehlen nachzuvollziehen, um in der Praxis zu sehen, wie ein Hugo-Blog von Grund auf entsteht. Anschließend kannst du alles noch einmal wiederholen und Namen sowie Struktur ganz nach deinen eigenen Vorstellungen anpassen.

### Schritt 1: Das Fundament vorbereiten (Die Werkzeuge)

Damit unsere „Seitenfabrik“ funktioniert, benötigen wir drei Werkzeuge:

**Git:** Das Versionskontrollsystem.

**Go:** Die Programmiersprache, auf der unsere Engine basiert.

**Hugo (Extended-Version):** Unser Static Site Generator.

#### Option A: Installation über das Terminal (Empfohlen)

Unter Windows (PowerShell als Administrator):

PowerShell

```powershell
winget install Git.Git
winget install GoLang.Go
winget install Hugo.Hugo.Extended
```

Unter Linux (Ubuntu/Debian):

```bash
sudo apt update
sudo apt install git golang hugo
```

Unter macOS (Homebrew):

```bash
brew install git go hugo
```

#### Option B: Manuelle Installation (Grafische Benutzeroberfläche)

Lade Git und Go herunter und installiere sie, indem du in den Standard-Installationsassistenten einfach auf **„Weiter“** klickst.

Für Hugo öffnest du die Releases des offiziellen GitHub-Projekts und lädst die passende `.zip`-Datei für dein Betriebssystem herunter (achte darauf, die Version mit `extended` im Namen zu wählen).

Entpacke die Dateien in einen sicheren Ordner (z. B. `C:\Hugo\`).

Unter Windows: Suche im Startmenü nach „Systemumgebungsvariablen bearbeiten“. Öffne „Umgebungsvariablen“, doppelklicke auf `Path` und füge den Pfad des Ordners hinzu, in dem du Hugo gespeichert hast.

Schließe anschließend das Terminal, öffne es erneut und gib `hugo version` ein. Wird die Versionsnummer angezeigt, ist alles bereit.

### Schritt 2: Das Fundament des Projekts

{{< figure src="Imagens-Tutorial-Hugo/01-VS-Code-Inicial.jpeg" title="Anfangsansicht von VS Code (Code-Editor) mit geöffnetem Projektordner" alt="Anfangsansicht von VS Code (Code-Editor) mit geöffnetem Projektordner." >}}

Öffne dein Terminal in dem Ordner, in dem du dein Projekt speichern möchtest, und gib Folgendes ein:

```bash
hugo new site meu-blog
cd meu-blog
```

{{< figure src="Imagens-Tutorial-Hugo/02-Criando-o-site.jpeg" title="Erste Terminalbefehle in VS Code zur Initialisierung eines Hugo-Blogs" alt="Erste Terminalbefehle in VS Code zur Initialisierung eines Hugo-Blogs." >}}

**Achtung:** Der Befehl `cd meu-blog` ist entscheidend. Er stellt sicher, dass sich dein Terminal innerhalb des Projektordners befindet. Alle folgenden Schritte müssen von dort aus ausgeführt werden.

### Schritt 3: Die Basis und Mehrsprachigkeit konfigurieren

Bereiten wir das Projekt darauf vor, mehrere Sprachen mithilfe der Verzeichnismethode zu unterstützen.

Öffne die Datei `hugo.toml` und ersetze ihren Inhalt durch Folgendes (vergiss nicht, die `baseURL` durch deinen GitHub-Benutzernamen zu ersetzen):

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

{{< figure src="Imagens-Tutorial-Hugo/04-Hugo-TOML.jpeg" title="Angepasste hugo.toml-Datei" alt="Angepasste hugo.toml-Datei." >}}

Erstelle anschließend im Terminal die Basisordner für die portugiesischen Inhalte:

```bash
mkdir -p content/pt-br/posts
mkdir -p content/pt-br/about
```

{{< figure src="Imagens-Tutorial-Hugo/05-Pastas-Base-PTBR.jpeg" title="Befehle zum Erstellen der Basisordner für portugiesische Inhalte" alt="Befehle zum Erstellen der Basisordner für portugiesische Inhalte." >}}

{{< figure src="Imagens-Tutorial-Hugo/06-Estruturas-Pastas-Base-PTBR.jpeg" title="Deine Ordnerstruktur sollte nun so aussehen" alt="Abbildung der Ordnerstruktur nach den letzten Befehlen." >}}

### Schritt 4: Das Layout erstellen (Die Vorlage)

Jetzt erstellen wir die HTML-Strukturen, welche unsere Inhalte aufnehmen und bereits die Logik für den Dark Mode sowie das seitliche Inhaltsverzeichnis enthalten. Erstelle die folgenden Dateien im Ordner `layouts/`:

**1. Das Grundgerüst (`layouts/baseof.html`):**

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
                <a href="{{ "/posts/" | relLangURL }}">Artikel</a>
                <a href="{{ "/about/" | relLangURL }}">Über mich</a>
                <button onclick="toggleTheme()" class="theme-btn" aria-label="Thema wechseln">🌓</button>
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

{{< figure src="Imagens-Tutorial-Hugo/07-baseof-html.jpeg" title="Deine Ordnerstruktur sollte nun so aussehen" alt="Abbildung der Ordnerstruktur nach den letzten Befehlen." >}}

**2. Die Startseite (`layouts/home.html`):**

```html
{{ define "main" }}
<div class="container hero">
    <h2>Willkommen!</h2>
    <p>Ein minimalistischer Ort, um Ideen und Erkenntnisse festzuhalten.</p>
</div>
{{ end }}
```

{{< figure src="Imagens-Tutorial-Hugo/08-home-html.jpeg" title="Inhalt der Datei home.html und Ordnerstruktur" alt="Inhalt der Datei home.html und Ordnerstruktur." >}}

**3. Der einzelne Beitrag und die „Über mich“-Seite (`layouts/single.html`):**

Hier fügen wir das `TableOfContents` ein, um automatisch ein dynamisches Inhaltsverzeichnis zu erzeugen.

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
            <span class="toc-title">In diesem Beitrag</span>
            {{ .TableOfContents }}
        </div>
    </aside>
</div>
{{ end }}
```

{{< figure src="Imagens-Tutorial-Hugo/09-single-html.jpeg" title="Inhalt der Datei single.html und Ordnerstruktur" alt="Inhalt der Datei single.html und Ordnerstruktur." >}}

**4. Die Beitragsliste (`layouts/list.html`):**

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

{{< figure src="Imagens-Tutorial-Hugo/10-list-html.jpeg" title="Inhalt der Datei list.html und Ordnerstruktur" alt="Inhalt der Datei list.html und Ordnerstruktur." >}}

### Schritt 5: Der Anstrich (Das professionelle CSS)

Hugo erstellt automatisch einen Ordner `static/` im Stammverzeichnis des Projekts. Öffne ihn, erstelle darin den Unterordner `css/` und lege anschließend eine Datei namens `custom.css` an.

Öffne `static/css/custom.css` und füge den folgenden Code ein. Er enthält automatische Variablen für den Dark Mode und gestaltet die seitliche Navigationsleiste.

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


{{< figure src="Imagens-Tutorial-Hugo/11-css-custom.jpeg" title="Inhalt der Datei custom.css und Ordnerstruktur" alt="Inhalt der Datei custom.css und Ordnerstruktur." >}}


### Schritt 6: Inhalte erstellen

**1. Die Seite „Über mich“**

Erstelle im Terminal die Seite „Über mich“:

```bash
hugo new content content/pt-br/about/index.md
```

{{< figure src="Imagens-Tutorial-Hugo/12-about-page.jpeg" title="Befehle zum Erstellen der Seite „Über mich“" alt="Befehle zum Erstellen der Seite „Über mich“" >}}

Bearbeite anschließend die neu erstellte Datei `content/pt-br/about/index.md`:

```markdown
---
title: "Über mich"
slug: "about"
isAbout: true
translationKey: "about"
---

Hallo! Ich bin ein Entwickler und teile in diesem Blog meinen Werdegang, meine Studien und meine Ideen.
```

(Hinweis: Wir verwenden `isAbout: true`, damit unser HTML-Layout erkennt, dass auf dieser Seite kein Datum angezeigt werden soll.)

{{< figure src="Imagens-Tutorial-Hugo/13-index-about.jpeg" title="Anfangszustand der index-Datei der Seite „Über mich“" alt="Anfangszustand der index-Datei der Seite „Über mich“" >}}

{{< figure src="Imagens-Tutorial-Hugo/14-pagina-sobre-editada.jpeg" title="Die Seite „Über mich“ nach den Änderungen" alt="Die Seite „Über mich“ nach den Änderungen" >}}

**2. Der erste Beitrag**

```bash
hugo new content content/pt-br/posts/meu-primeiro-artigo/index.md
```

{{< figure src="Imagens-Tutorial-Hugo/15-my-first-article.jpeg" title="Befehl zum Erstellen der Datei für den ersten Artikel" alt="Befehl zum Erstellen der Datei für den ersten Artikel" >}}

{{< figure src="Imagens-Tutorial-Hugo/16-estado-inicial-primeiro-artigo-md.jpeg" title="Anfangszustand der Markdown-Datei des ersten Artikels" alt="Anfangszustand der Markdown-Datei des ersten Artikels" >}}

Bearbeite anschließend die Datei `content/pt-br/posts/meu-primeiro-artigo/index.md`:

```markdown
---
title: "Mein erster Artikel"
slug: "mein-erster-artikel"
date: 2026-06-09T10:00:00-03:00
draft: false
translationKey: "primeiro-artigo"
---

Im Folgenden betrachten wir die Vorteile statischer Websites.

## Absolute Geschwindigkeit

Hugo kompiliert Seiten innerhalb von Millisekunden.

## Sicherheit

Da keine Datenbank in Echtzeit betrieben wird, sinkt das Risiko eines Angriffs erheblich.
```

{{< figure src="Imagens-Tutorial-Hugo/17-primeiro-artigo-pos-alteracoes.jpeg" title="Zustand der Markdown-Datei des ersten Artikels nach den Änderungen" alt="Zustand der Markdown-Datei des ersten Artikels nach den Änderungen" >}}

Starte anschließend im Terminal den Entwicklungsserver:

```bash
hugo server
```

{{< figure src="Imagens-Tutorial-Hugo/18-hugo-server-comando.jpeg" title="Der Befehl hugo server im Terminal" alt="Der Befehl hugo server im Terminal" >}}

Öffne anschließend im Browser `http://localhost:1313`. Du solltest nun das fertige Blog-Grundgerüst sehen. Das Design ist noch sehr schlicht, aber keine Sorge – du kannst es später ganz nach deinen Vorstellungen anpassen.

{{< figure src="Imagens-Tutorial-Hugo/19-modelo-blog-pronto.jpeg" title="Fertiges Blog-Grundmodell" alt="Fertiges Blog-Grundmodell" >}}

### Schritt 7: Kostenloses Deployment (GitHub Actions)

Jetzt bringen wir das Ganze kostenlos online.

Bevor wir den Code hochladen, müssen wir Git mitteilen, dass temporäre Dateien und der endgültig generierte Website-Ordner nicht gespeichert werden sollen (da der GitHub-Roboter dies für uns übernimmt). Erstelle im Stammverzeichnis deines Projekts eine Datei namens `.gitignore` und füge Folgendes ein:

```plaintext
public/
resources/
.hugo_build.lock
```

Nur damit du verstehst, was jede Zeile bedeutet:

`public/`: Dies ist der Ordner, in den Hugo die fertigen statischen HTML-Dateien schreibt, wenn du den Build-Befehl ausführst. Da wir GitHub Actions verwenden, um diesen Build direkt auf den GitHub-Servern auszuführen, müssen wir den auf unserem eigenen Rechner erzeugten Ordner nicht hochladen.

`resources/`: Hugo erstellt diesen Ordner als temporären Cache, um Bilder und Stylesheets bei zukünftigen Builds schneller zu verarbeiten. Er muss nicht versioniert werden.

`.hugo_build.lock`: Dies ist eine kleine Sperrdatei, die Hugo erstellt, um zu verhindern, dass zwei Build-Prozesse gleichzeitig laufen. Im Repository hat sie keinen Nutzen.

Erstelle nun über den grünen Button ein leeres Repository auf GitHub.

{{< figure src="Imagens-Tutorial-Hugo/19.1-new-repository.png" title="Schaltfläche „New“" alt="Schaltfläche „New“ zum Erstellen eines neuen GitHub-Repositories" >}}

Vergiss nicht festzulegen, ob dein Repository öffentlich oder privat sein soll. Ist es öffentlich, kann jeder die Dateien sehen, die du dorthin hochlädst. Ist es privat, können nur du oder Personen mit entsprechender Berechtigung darauf zugreifen.

{{< figure src="Imagens-Tutorial-Hugo/20-github-repositorio.jpeg" title="Repository wird auf GitHub erstellt" alt="Repository wird auf GitHub erstellt" >}}

Initialisiere anschließend in deinem Terminal (immer noch innerhalb des Ordners `meu-blog`) Git:

```bash
git init
git add .
git commit -m "commit inicial"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
git push -u origin main
```

{{< figure src="Imagens-Tutorial-Hugo/21-git-init.jpeg" title="Befehle zum Initialisieren von Git" alt="Befehle zum Initialisieren von Git" >}}

{{< figure src="Imagens-Tutorial-Hugo/22-commit-inicial.jpeg" title="Befehle für den ersten Commit auf GitHub" alt="Befehle für den ersten Commit auf GitHub" >}}

{{< figure src="Imagens-Tutorial-Hugo/23-git-remote.jpeg" title="Befehl zum Umbenennen des Branches (Konvention) und Verbinden des Terminals mit dem GitHub-Repository" alt="Befehl zum Umbenennen des Branches (Konvention) und Verbinden des Terminals mit dem GitHub-Repository" >}}

{{< figure src="Imagens-Tutorial-Hugo/24-primeiro-push.jpeg" title="Befehl zum erstmaligen Hochladen des Codes auf GitHub" alt="Befehl zum erstmaligen Hochladen des Codes auf GitHub" >}}

Gehe anschließend auf GitHub zur Registerkarte **Settings > Pages**. Wähle unter **Source** die Option **GitHub Actions** aus.

{{< figure src="Imagens-Tutorial-Hugo/25-github-settings.jpeg" title="Settings bzw. Einstellungen im Projekt-Repository auf GitHub öffnen" alt="Hinweis, wo sich Settings bzw. Einstellungen im Projekt-Repository auf GitHub befinden" >}}

{{< figure src="Imagens-Tutorial-Hugo/26-source-github-pages.jpeg" title="Source auf GitHub Actions setzen" alt="Hinweis, wo Source auf GitHub Actions gesetzt wird" >}}

Wechsle anschließend zurück zu deinem Code-Editor. Erstelle innerhalb deines Projekts (`meu-blog`) einen Ordner namens `.github`. Darin erstellst du einen weiteren Ordner `workflows` und schließlich darin eine Datei namens `hugo.yaml` (der vollständige Pfad muss also `meu-blog/.github/workflows/hugo.yaml` lauten).

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

{{< figure src="Imagens-Tutorial-Hugo/27-workflows.jpeg" title="Ordnerstruktur: workflows innerhalb von .github mit der Datei hugo.yaml" alt="Ordnerstruktur: workflows innerhalb von .github mit der Datei hugo.yaml" >}}

Führe anschließend im Terminal `git add .`, danach den `commit` und schließlich den `push` aus. Der GitHub-Roboter wird „aufwachen“, deine Website kompilieren und sie unter deiner offiziellen Adresse veröffentlichen.

{{< figure src="Imagens-Tutorial-Hugo/28-segundo-push.jpeg" title="Befehle zum Hochladen der Änderungen in das GitHub-Repository" alt="Befehle zum Hochladen der Änderungen in das GitHub-Repository" >}}

Wenn du im GitHub-Repository die Registerkarte **Actions** öffnest, kannst du den Build-Prozess live verfolgen – gelb bedeutet, dass er gerade läuft, grün bedeutet, dass er erfolgreich abgeschlossen wurde.

{{< figure src="Imagens-Tutorial-Hugo/30-GitHubActions-building.jpeg" title="GitHub Actions während des Build-Vorgangs" alt="GitHub Actions während des Build-Vorgangs" >}}

{{< figure src="Imagens-Tutorial-Hugo/31-GitHubActions-Estado-Final.png" title="GitHub Actions nach erfolgreichem Abschluss des Builds" alt="GitHub Actions nach erfolgreichem Abschluss des Builds" >}}

Jedes Mal, wenn du einen neuen `git push` ausführst, um Änderungen auf GitHub hochzuladen (zum Beispiel nachdem du einen neuen Markdown-Beitrag fertiggestellt hast), kümmert sich GitHub Actions automatisch darum, dein Blog in der neuesten Version zu veröffentlichen.

Vergiss jedoch nicht, vorher `git add` auszuführen, um deine Dateien zur sogenannten *Staging Area* hinzuzufügen, und anschließend `git commit`, um die Änderungen zu speichern. Andernfalls wird nichts an GitHub übertragen.

Nach Abschluss dieses Tutorials solltest du eine sehr einfache Grundstruktur besitzen. Ich empfehle dir, eine KI um Unterstützung zu bitten, um das Design weiter zu verbessern und an deine eigenen Vorstellungen anzupassen. Das Wichtigste – nämlich die grundlegende Struktur – ist bereits erledigt.

## Nützliche Ressourcen

In der offiziellen Hugo-Dokumentation findest du viele weitere Informationen:

https://gohugo.io/

Vor allem der Bereich **Docs** enthält zahlreiche wichtige Informationen.

Ein Video, das mir beim Einstieg in Hugo sehr geholfen hat, stammt von **Fireship** (englischsprachiger Inhalt):

{{< youtube 0RKpf3rK57I >}}

Ich empfehle außerdem das Video von **NetworkChuck** (ebenfalls auf Englisch), in dem gezeigt wird, wie man ein Blog mit Hugo erstellt, ein Theme einbindet und **Obsidian** zum Schreiben der Beiträge verwendet.

{{< youtube dnE7c0ELEH8 >}}

## Abschluss

Wie ich bereits im ersten Beitrag dieses Blogs erwähnt habe, ist eines meiner Ziele, meine Schreibfähigkeiten kontinuierlich zu verbessern. Ich weiß, dass dieser Artikel noch lange nicht das Niveau erreicht, das ich mir wünsche, aber ich denke, dass er bereits eine brauchbare Einführung für alle bietet, die Hugo kennenlernen möchten.

Ich möchte dich ausdrücklich dazu ermutigen, die offizielle Dokumentation zu lesen, denn dort findest du letztlich alles, was du brauchst. Falls du kein Englisch sprichst, kannst du Werkzeuge wie Google Translate verwenden oder – noch besser – eine KI darum bitten, dir die Dokumentation verständlich in deiner Muttersprache zu erklären.

Ich weiß selbst noch sehr wenig über Hugo und lerne ständig dazu, während ich diese Beiträge schreibe. Wenn ich Lust dazu habe, werde ich vielleicht weitere Artikel über interessante Funktionen und Möglichkeiten veröffentlichen, die dieses Werkzeug bietet – zum Beispiel über die Logik hinter dem **Table of Contents** (dieses Navigationsmenü mit den Überschriften auf der Seite), das ich beim Schreiben dieses Beitrags in meinem eigenen Blog implementiert habe.
