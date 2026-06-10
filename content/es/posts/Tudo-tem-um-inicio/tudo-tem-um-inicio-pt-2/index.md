---
title: "Todo tiene un inicio — Parte 2"
slug: "todo-tiene-un-inicio-parte-2"
date: 2026-06-09T10:12:00-03:00
draft: false
categories: ["Inicio"]
series: ["Todo Tiene un Inicio"]
description: "La segunda parte de la construcción de MitzIdeas, centrada en la instalación y configuración de Hugo."
translationKey: "primeiro-post-parte-2"
---

## Introducción

En el post anterior, compartí la idea detrás del nacimiento de este blog. Hoy vamos a abrir el capó y hablar sobre la ingeniería que mantiene MitzIdeas funcionando sin costarme un solo centavo en hosting, manteniendo una velocidad absurda y control total sobre cada píxel.

Intentaré hablar un poco más sobre toda la estructura que sostiene este blog, explicar mejor cada concepto y, al final, ofrecer un mini tutorial para ayudarte a ir de cero a tu primer blog hecho con Hugo.

---

## El Motor: Entendiendo Hugo

Hugo es un **Generador de Sitios Estáticos (Static Site Generator, SSG)** escrito en el lenguaje de programación **Go**. Fue creado por **Steve Francia** (conocido como *spf13* en el ecosistema de Golang) en 2013, quien diseñó la estructura base de cómo el proyecto procesa los templates y la famosa lógica de *lookup order*.

Alrededor de 2015, el liderazgo pasó a **Bjorn Erik Pedersen** (*bep*), quien transformó este motor en un verdadero auto de carreras profesional. Entre los logros más destacados de Bjorn:

* **Rendimiento Extremo:** Optimización del compilador para aprovechar al máximo los procesadores multi-core, haciendo que sitios con decenas de miles de páginas se compilen en pocos segundos.
* **Hugo Pipes:** Procesamiento nativo de assets, minificación, fingerprinting y sólido soporte multilingüe (`i18n`).
* **Comunidad:** Una dedicación casi sobrehumana para responder issues y revisar pull requests en GitHub, manteniendo el proyecto a la vanguardia de la Web.

Gracias a este framework flexible, Hugo es ampliamente utilizado para crear portales de documentación, sitios gubernamentales, corporativos y, por supuesto, blogs personales. Actúa como un excelente **Content Management System (CMS)** basado en archivos, donde tu editor de código (como VS Code) se convierte en tu panel de control.

---

## ¿Por qué usar Hugo en lugar de HTML, CSS y JavaScript puros?

Si construyes un sitio solo con HTML, CSS y JS puro (el famoso *Vanilla*), estás poniendo ladrillos uno por uno. Funciona de maravilla para 3 o 5 páginas. Pero ¿qué pasa cuando tienes 100 posts?

Imagina que tengo 50 posts y decido cambiar el enlace de la página "Sobre" en el encabezado:
* **En HTML puro:** Tendría que abrir **51 archivos** (los 50 posts + la página `index.html`) y cambiarlos manualmente uno por uno. Si me olvido de uno, generé una inconsistencia.
* **En Hugo:** Cambio un único archivo parcial (`layouts/_partials/header.html`) y, al correr el build, Hugo inyecta ese cambio en todas las páginas al instante. Hugo es una **fábrica de páginas**.

Además, escribir en Markdown (`.md`) es infinitamente más productivo que armar etiquetas HTML como piezas de LEGO. Te enfocas en el texto y el motor se encarga del resto.

El ecosistema también trae ventajas como la **Minificación** (eliminación de espacios en blanco del código) y el **Fingerprinting** (técnica de cache busting que agrega un hash único al nombre del archivo, como `custom.min.8723.css`, forzando al navegador del usuario a descargar la versión más nueva e ignorar el caché antiguo).

### ¿Cuándo NO usar Hugo?
No estoy aquí para vender Hugo como una bala de plata. Es excesivo para landing pages simples de una sola página. Tampoco es ideal para sistemas que necesitan datos procesados en tiempo real en el servidor, como paneles con login de usuarios o feeds dinámicos de redes sociales. En esos escenarios, frameworks como Django (que uso en mi día a día) siguen siendo la elección correcta.

---

## Características Fundamentales de Arquitectura

### 1. Servidor de Desarrollo Integrado
Cuando escribes `hugo server` en el terminal, levanta un servidor local que crea un "túnel" entre tus archivos y el navegador. Cada vez que guardas un archivo, detecta los cambios y recarga la página en milisegundos.

Como Hugo está hecho en Go, tenemos acceso a los **Go Templates** para interpolar y transformar datos dinámicos. Quien viene del ecosistema Python/Django notará una similitud inmediata. Es un entorno totalmente *batteries included*.

### 2. Jerarquía de Layouts
Hugo trabaja con moldes. Defines un archivo principal llamado `baseof.html` con el esqueleto del sitio (lo que se repite en todo, como el header y footer) y abres un "hueco" usando el comando `{{ block "main" . }}{{ end }}`. Los demás templates simplemente heredan ese molde y rellenan el hueco.

### 3. Taxonomías Nativas
Clasificar contenido en Hugo es extremadamente poderoso y no requiere plugins. El sistema de **taxonomías** crea relaciones lógicas complejas (como tags, categorías y series), generando un índice invertido y ponderado. Sabe exactamente qué posts están relacionados entre sí sin necesidad de hacer una sola query SQL.

### 4. Diccionario de Archivos Extraños
* `archetypes/`: Guarda tus templates de borrador. El archivo `default.md` define qué campos vendrán prellenados cada vez que crees un post nuevo.
* `.toml`: El formato de archivo de configuración (como el `hugo.toml`). Es el cerebro global del sitio.
* `single.html` vs `list.html`: El primero dicta el visual de un post único; el segundo renderiza páginas que listan múltiples posts (como los índices de categorías).

---

## El Modelo de Seguridad: "Confianza Restringida"

Hugo opera bajo una premisa muy clara: **confía en quien escribe el código (el desarrollador), pero no en quien escribe el contenido (el Markdown).** Su política nativa se basa en listas de permisos (allowlists). Por defecto, el acceso a comandos del sistema operativo (`os/exec`), comunicaciones remotas y el uso de formatos de contenido como HTML puro (`allowContent`) están completamente bloqueados para evitar ataques de inyección de scripts maliciosos (XSS). Solo ejecuta herramientas externas que tú autorices explícitamente, como `tailwindcss` o `postcss`.

Y como el resultado final publicado son solo archivos estáticos guardados en disco, **el 99% de las vulnerabilidades tradicionales de servidores (como SQL Injection) simplemente dejan de existir.**

---

## Cambios en la versión 0.146.0 que me dieron dolor de cabeza

El equipo de desarrollo de Hugo rehízo completamente el sistema de procesamiento de templates en la versión `v0.146.0`. Esto rompió la compatibilidad con casi todos los tutoriales más antiguos de internet. Si lees un tutorial que te mande a crear la carpeta `_default` o el archivo `index.html`, sabe que está desactualizado.

Esto es lo que cambió y lo que debes aplicar en el patrón moderno:
* **Fin de la carpeta `_default`:** Los layouts por defecto ahora viven directamente en la raíz de la carpeta `layouts/`.
* **Carpetas de sistema con `_`:** Las carpetas internas del motor ahora requieren un guión bajo al frente. Por lo tanto, `partials/` pasó a ser `_partials/` y `shortcodes/` pasó a ser `_shortcodes/`.
* **Fin del `index.html` en la Home:** El archivo responsable de renderizar la página de inicio ahora debe llamarse obligatoriamente `home.html`.
* **Nomenclatura de Base:** Los identificadores de templates base cambiaron el guión por el punto. El antiguo `list-baseof.html` ahora debe renombrarse a `baseof.list.html`.

Puedes consultar (y lo recomiendo ampliamente) los cambios realizados en los templates en la [Documentación Oficial](https://gohugo.io/templates/new-templatesystem-overview/).

---

## RSS en Hugo: Autonomía para el Lector

El **RSS (Really Simple Syndication)** es el "Twitter de los viejos tiempos" que aún sobrevive como una herramienta fantástica. Genera un archivo en formato `XML` que contiene un resumen estructurado de tus novedades (título, enlace, fecha).

Con él, el lector no depende del algoritmo de ninguna gran corporación tecnológica para ver tu contenido; basta con que se suscriba a tu feed en lectores como Feedly o Inoreader para ser notificado al instante. Además, puedes conectar herramientas como Zapier o IFTTT a tu RSS para automatizar publicaciones en LinkedIn o avisos en Discord cada vez que salga un post. Hugo genera este archivo (`index.xml`) de forma totalmente nativa.

---

## Globalizando el Blog con el Modo Multilingüe

Hugo gestiona múltiples idiomas de forma espectacular a través de tablas de traducción. Existen dos métodos para gestionar tus archivos: por **Sufijos** (ej: `post.en.md`, `post.pt-br.md`) o por **Directorios**.

### Sufijos vs. Directorios: Aprende de mi error
Inicialmente adopté el método de sufijos porque parecía más simple. Pero en el momento en que decidí organizar mis posts en capas y series profundas (como `content/posts/Libros/Filosofia/`), mezclar archivos de cinco idiomas diferentes dentro de la misma carpeta convirtió el proyecto en un caos absoluto.

**La Solución:** Migré a la traducción por **Directorios**. En el `hugo.toml`, configuré carpetas aisladas para cada idioma (`content/pt-br/`, `content/en/`, etc.). Ahora, los archivos Markdown de todos los idiomas se llaman simplemente `index.md`, manteniendo el árbol de directorios perfectamente limpio.

Para vincular las páginas, usamos el atributo `translationKey` en el encabezado del post. No importa si la carpeta en portugués se llama `Tudo-tem-um-inicio` y la del francés `Tout-a-un-debut`; si ambas tienen el mismo `translationKey`, Hugo crea el vínculo en el selector de idiomas del menú de forma automática.

Para separar las cadenas de interfaz (como los botones "Volver" o "Buscar"), usamos la carpeta `i18n/` con archivos `.toml` para cada idioma, llamándolos en el HTML a través de la función `{{ T "clave_de_cadena" }}`.

En esta página de la [Documentación Oficial de Hugo](https://gohugo.io/content-management/multilingual/) puedes leer más sobre cómo manejar diferentes idiomas.

---

## URLs Limpias: El Secreto de los Slugs y Permalinks

Por defecto, Hugo refleja el camino físico de tus carpetas directamente en la URL del sitio. Esto genera enlaces largos y redundantes. Peor aún: si mantienes el nombre de tus carpetas en portugués para tu propia organización mental, tu URL en inglés terminaría heredando el término en portugués.

Para resolver esto, usamos dos propiedades:
1. **Slugs:** En el Front Matter de cada archivo, definimos el parámetro `slug = "nombre-limpio-de-url"`. Obliga a la URL de ese post a adoptar un término específico y completamente en minúsculas.
2. **Permalinks:** En el archivo `hugo.toml`, definimos una regla global para centralizar las rutas, limpiando los caminos intermedios de las carpetas:

```toml
[permalinks]
  posts = "/posts/:slug/"
```

Esto garantiza que, sin importar en qué subcarpeta esté escondido el Markdown, la URL final generada para el usuario será corta, elegante y amigable para el SEO.

### GitHub Pages y GitHub Actions
Para poner el blog en línea, combiné dos herramientas gratuitas de GitHub:

**GitHub Pages**: Funciona como nuestro hosting estático. No procesa lenguajes de servidor; actúa simplemente como un escaparate que muestra los archivos HTML, CSS y JS puros resultantes del build.

**GitHub Actions**: Como nuestro repositorio solo guarda los archivos de desarrollo (Markdowns y layouts), necesitamos a alguien que compile el sitio. Actions es una máquina virtual (un robot corriendo Linux Ubuntu) que está en espera. Cada vez que hago un *git push main*, ese robot "despierta", lee nuestro archivo de instrucciones `hugo.yaml`, instala la versión correcta de **Hugo Extended**, compila todo el proyecto y entrega la carpeta `public/` lista para que GitHub Pages la muestre al mundo.

## Mini Tutorial: Creando tu Propio Blog con Hugo
Pongamos manos a la obra y creemos un blog estructurado desde cero, aplicando todas las reglas modernas de la versión 0.146.0+ (no hay razón para no empezar tu blog en la versión más actualizada).
Te recomiendo que sigas el tutorial con los comandos exactos que proporcioné solo para ver en la práctica cómo se crea un blog desde cero con Hugo. Después de eso, puedes rehacerlo adaptando los nombres y todo lo demás a tu gusto.

### Paso 1: Preparando el Terreno (Las Herramientas)
Para que nuestra "fábrica" de páginas funcione, necesitamos tres herramientas:

**Git:** El sistema de versionado de código.

**Go:** El lenguaje base de nuestro motor.

**Hugo (Versión Extended):** Nuestro generador de sitios estáticos.



#### Opción A: Instalación vía Terminal (Recomendado)
En Windows (PowerShell como Administrador):

PowerShell
```powershell
winget install Git.Git 
winget install GoLang.Go 
winget install Hugo.Hugo.Extended
```

En Linux (Ubuntu/Debian):

```bash
sudo apt update 
sudo apt install git golang hugo
```

En macOS (Homebrew):
```bash
brew install git go hugo
```

#### Opción B: Instalación Manual (Interfaz Gráfica)
Descarga e instala Git y Go haciendo clic en "Siguiente" en los instaladores estándar.

Para Hugo, ve a las Releases en el GitHub oficial del proyecto y descarga la versión `.zip` correspondiente a tu sistema operativo (asegúrate de descargar la versión con *extended* en el nombre).

Extrae los archivos en una carpeta segura (ej: `C:\Hugo\`).

En Windows: Busca en el menú inicio "Editar las variables de entorno del sistema". Haz clic en "Variables de entorno", doble clic en Path y agrega la ruta de la carpeta donde pusiste Hugo.

Cierra el terminal, ábrelo nuevamente y escribe `hugo version`. Si aparece la versión, estamos listos.

### Paso 2: La Fundación del Proyecto

{{< figure src="Imagens-Tutorial-Hugo/01-VS-Code-Inicial.jpeg" title="Vista inicial de VS Code (editor de código) con la carpeta del proyecto abierta" alt="Vista inicial de VS Code (editor de código) con la carpeta del proyecto abierta." >}}

Abre tu terminal en la carpeta donde deseas guardar el proyecto y escribe:

``` bash
hugo new site meu-blog
cd meu-blog
```

{{< figure src="Imagens-Tutorial-Hugo/02-Criando-o-site.jpeg" title="Vista inicial de los primeros comandos en el terminal de VS Code que inicializan el proyecto del blog hecho con Hugo" alt="Vista inicial de los primeros comandos en el terminal de VS Code que inicializan el proyecto del blog hecho con Hugo." >}}

**Atención:** El comando `cd meu-blog` es crucial. Garantiza que tu terminal esté dentro de la carpeta del proyecto. Todos los pasos siguientes deben ejecutarse desde aquí.

### Paso 3: Configurando la Base y el Multilingüismo
Vamos a preparar el proyecto para soportar múltiples idiomas usando la metodología de Directorios.

Abre el archivo `hugo.toml` y reemplaza todo por esto (no olvides cambiar la baseURL por tu nombre de usuario de GitHub):

```toml
baseURL = "https://TU-USUARIO.github.io/meu-blog/"
title = "Mi Blog Profesional"
defaultContentLanguage = "pt-br"

[permalinks]
  posts = "/posts/:slug/"

[languages]
  [languages."pt-br"]
    languageName = "Português"
    weight = 1
    contentDir = "content/pt-br"
```
{{< figure src="Imagens-Tutorial-Hugo/04-Hugo-TOML.jpeg" title="Archivo hugo.toml adaptado" alt="Archivo hugo.toml adaptado." >}}


En el terminal, crea las carpetas base para los contenidos en portugués:

```bash
mkdir -p content/pt-br/posts
mkdir -p content/pt-br/about
```

{{< figure src="Imagens-Tutorial-Hugo/05-Pastas-Base-PTBR.jpeg" title="Comandos para crear las carpetas base para los contenidos en portugués" alt="Comandos para crear las carpetas base para los contenidos en portugués." >}}
{{< figure src="Imagens-Tutorial-Hugo/06-Estruturas-Pastas-Base-PTBR.jpeg" title="Tu estructura de carpetas debe verse así" alt="Imagen de la estructura de carpetas después de los últimos comandos." >}}


### Paso 4: Construyendo el Layout (El Molde)
Vamos a crear las estructuras HTML que recibirán nuestros textos, ya incorporando la lógica de Dark Mode y el Índice Lateral. Crea los archivos a continuación dentro de la carpeta `layouts/`:

**1. El Esqueleto (layouts/baseof.html):**

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
                <a href="{{ "/posts/" | relLangURL }}">Artículos</a>
                <a href="{{ "/about/" | relLangURL }}">Sobre mí</a>
                <button onclick="toggleTheme()" class="theme-btn" aria-label="Cambiar Tema">🌓</button>
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

{{< figure src="Imagens-Tutorial-Hugo/07-baseof-html.jpeg" title="Tu estructura de carpetas debe verse así" alt="Imagen de la estructura de carpetas después de los últimos comandos." >}}


**2. La Página de Inicio (layouts/home.html):**

```html
{{ define "main" }}
<div class="container hero">
    <h2>¡Bienvenido!</h2>
    <p>Un espacio minimalista para documentar ideas y aprendizajes.</p>
</div>
{{ end }}
```
{{< figure src="Imagens-Tutorial-Hugo/08-home-html.jpeg" title="Contenido del archivo home.html y estructura de carpetas" alt="Contenido del archivo home.html y estructura de carpetas." >}}


**3. El Post Individual y la Página Sobre mí (layouts/single.html):**
Aquí insertamos el TableOfContents para generar el índice dinámico.

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
            <span class="toc-title">En Este Post</span>
            {{ .TableOfContents }}
        </div>
    </aside>
</div>
{{ end }}
```
{{< figure src="Imagens-Tutorial-Hugo/09-single-html.jpeg" title="Contenido del archivo single.html y estructura de carpetas" alt="Contenido del archivo single.html y estructura de carpetas." >}}

**4. La Lista de Posts (layouts/list.html):**

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
{{< figure src="Imagens-Tutorial-Hugo/10-list-html.jpeg" title="Contenido del archivo list.html y estructura de carpetas" alt="Contenido del archivo list.html y estructura de carpetas." >}}

### Paso 5: La Pintura (El CSS Profesional)
Hugo generó automáticamente una carpeta `static/` en la raíz del proyecto. Entra en ella, crea una subcarpeta `css/` y un archivo llamado `custom.css`.

Abre `static/css/custom.css` y pega este código. Tiene variables automáticas para el Modo Oscuro y estiliza la barra lateral.

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
{{< figure src="Imagens-Tutorial-Hugo/11-css-custom.jpeg" title="Contenido del archivo custom.css y estructura de carpetas" alt="Contenido del archivo custom.css y estructura de carpetas." >}}

### Paso 6: Creando los Contenidos
**1. La Página "Sobre mí"**:
En el terminal, crea la página Sobre mí:

```bash
hugo new content content/pt-br/about/index.md
```
{{< figure src="Imagens-Tutorial-Hugo/12-about-page.jpeg" title="Comandos para crear la página Sobre mí" alt="Comandos para crear la página Sobre mí" >}}


Edita el archivo recién creado en content/pt-br/about/index.md:

```markdown
---
title: "Sobre mí"
slug: "about"
isAbout: true
translationKey: "about"
---

¡Hola! Soy un desarrollador compartiendo mi camino, estudios e ideas en este blog.
```

(Nota: usamos `isAbout: true` para que nuestro layout HTML sepa que no debe mostrar la fecha en esta página.)

{{< figure src="Imagens-Tutorial-Hugo/13-index-about.jpeg" title="Estado inicial del index de la página Sobre mí" alt="Estado inicial del index de la página Sobre mí" >}}

{{< figure src="Imagens-Tutorial-Hugo/14-pagina-sobre-editada.jpeg" title="Página Sobre mí después de las modificaciones" alt="Página Sobre mí después de las modificaciones" >}}

**2. El Primer Post:**

```bash
hugo new content content/pt-br/posts/meu-primeiro-artigo/index.md
```
{{< figure src="Imagens-Tutorial-Hugo/15-my-first-article.jpeg" title="Comando para crear el archivo del primer artículo" alt="Comando para crear el archivo del primer artículo" >}}

{{< figure src="Imagens-Tutorial-Hugo/16-estado-inicial-primeiro-artigo-md.jpeg" title="Estado inicial del markdown del primer artículo" alt="Estado inicial del markdown del primer artículo" >}}

Edita el archivo en content/pt-br/posts/meu-primeiro-artigo/index.md:

```markdown
---
title: "Mi Primer Artículo"
slug: "mi-primer-articulo"
date: 2026-06-09T10:00:00-03:00
draft: false
translationKey: "primeiro-artigo"
---

A continuación, exploramos las ventajas de generar sitios estáticos.

## Velocidad Absoluta
Hugo compila páginas en milisegundos.

## Seguridad
Como no hay base de datos corriendo en tiempo real, las probabilidades de intrusión se desploman.
```

{{< figure src="Imagens-Tutorial-Hugo/17-primeiro-artigo-pos-alteracoes.jpeg" title="Estado del markdown del primer artículo después de las modificaciones" alt="Estado del markdown del primer artículo después de las modificaciones." >}}


En el terminal, levanta el servidor:

```bash
hugo server
```
{{< figure src="Imagens-Tutorial-Hugo/18-hugo-server-comando.jpeg" title="Comando hugo server en el terminal" alt="Comando hugo server en el terminal" >}}

Abre el navegador en `http://localhost:1313`. Verás el modelo de blog listo. El visual es bastante simple, pero no te preocupes, puedes cambiarlo después.

{{< figure src="Imagens-Tutorial-Hugo/19-modelo-blog-pronto.jpeg" title="Modelo del blog listo" alt="Modelo del blog listo" >}}

### Paso 7: El Deploy Gratuito (GitHub Actions)

Pongamos esto en línea de forma gratuita.

Antes de enviar el código, necesitamos avisarle a Git que no guarde archivos temporales ni la carpeta del sitio final (ya que el robot de GitHub hará eso por nosotros). Crea un archivo llamado `.gitignore` en la raíz de tu proyecto y pega lo siguiente:
```plaintext
public/
resources/
.hugo_build.lock
```

Para que entiendas cada línea:

`public/`: Esta es la carpeta donde Hugo vuelca los archivos HTML estáticos finales cuando ejecutas el comando de compilación. Como usamos GitHub Actions para compilar directamente en sus servidores, no necesitamos enviar la carpeta generada en nuestra máquina.

`resources/`: Hugo crea esta carpeta como caché temporal para procesar imágenes y estilos más rápido en las próximas ejecuciones. No necesita ser versionada.

`.hugo_build.lock`: Es un pequeño archivo de bloqueo de seguridad creado por Hugo para evitar que dos instancias ejecuten el build al mismo tiempo. No sirve de nada en el repositorio.

Crea un repositorio vacío en GitHub haciendo clic en el botón verde.

{{< figure src="Imagens-Tutorial-Hugo/19.1-new-repository.png" title="Botón New" alt="Botón New para crear un nuevo repositorio en GitHub" >}}

No olvides configurar si quieres que tu repositorio sea público o privado. Si es público, cualquier persona puede acceder a los archivos que envíes a ese repositorio en GitHub. Si es privado, solo tú o quienes tengan permiso pueden verlos.

{{< figure src="Imagens-Tutorial-Hugo/20-github-repositorio.jpeg" title="Repositorio siendo creado en GitHub" alt="Repositorio siendo creado en GitHub" >}}

En tu terminal (aún dentro de la carpeta meu-blog), inicia Git:

```bash
git init
git add .
git commit -m "commit inicial"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/TU-REPOSITORIO.git
git push -u origin main
```

{{< figure src="Imagens-Tutorial-Hugo/21-git-init.jpeg" title="Comandos para iniciar Git" alt="Comandos para iniciar Git" >}}

{{< figure src="Imagens-Tutorial-Hugo/22-commit-inicial.jpeg" title="Comandos para el primer guardado de código en GitHub" alt="Comandos para el primer guardado de código en GitHub" >}}

{{< figure src="Imagens-Tutorial-Hugo/23-git-remote.jpeg" title="Comando para renombrar tu rama (convención) y conectar tu terminal al repositorio en GitHub" alt="Comando para renombrar tu rama (convención) y conectar tu terminal al repositorio en GitHub" >}}

{{< figure src="Imagens-Tutorial-Hugo/24-primeiro-push.jpeg" title="Comando para enviar tu código a GitHub por primera vez" alt="Comando para enviar tu código a GitHub por primera vez" >}}


En GitHub, ve a la pestaña **Settings > Pages**. En **Source**, elige **GitHub Actions**.
{{< figure src="Imagens-Tutorial-Hugo/25-github-settings.jpeg" title="Acceder a Settings en GitHub desde el repositorio de tu proyecto" alt="Indicación de dónde acceder a Settings en GitHub desde el repositorio de tu proyecto" >}}

{{< figure src="Imagens-Tutorial-Hugo/26-source-github-pages.jpeg" title="Definir Source para usar GitHub Actions" alt="Indicación de dónde definir Source para usar GitHub Actions" >}}

Vuelve a tu editor de código. Dentro de la carpeta de tu proyecto (meu-blog), crea una carpeta llamada `.github`. Dentro de ella, crea otra llamada `workflows`, y allí dentro crea un archivo llamado `hugo.yaml` (la ruta final exacta debe ser `meu-blog/.github/workflows/hugo.yaml`).

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

{{< figure src="Imagens-Tutorial-Hugo/27-workflows.jpeg" title="Estructura de carpetas: carpeta workflows dentro de la carpeta .github con el archivo hugo.yaml" alt="Estructura de carpetas: carpeta workflows dentro de la carpeta .github con el archivo hugo.yaml" >}}

Haz `git add .` en el terminal, luego el `commit` y el `push`. El robot va a "despertar", compilar tu sitio y dejarlo público en tu dirección oficial.
{{< figure src="Imagens-Tutorial-Hugo/28-segundo-push.jpeg" title="Comandos para enviar los cambios al repositorio de GitHub" alt="Comandos para enviar los cambios al repositorio de GitHub" >}}

Si vas a la pestaña **Actions** de GitHub, puedes ver el proceso de build corriendo (cuando está en amarillo) o completado (cuando está en verde).

{{< figure src="Imagens-Tutorial-Hugo/30-GitHubActions-building.jpeg" title="GitHub Actions con build en curso" alt="GitHub Actions con build en curso" >}}

{{< figure src="Imagens-Tutorial-Hugo/31-GitHubActions-Estado-Final.png" title="GitHub Actions con build finalizado" alt="GitHub Actions con el build finalizado" >}}

Cada vez que hagas un nuevo *git push* para enviar los cambios a GitHub (por ejemplo, cuando terminas un nuevo archivo markdown, que es un post), GitHub Actions se encarga de publicar la versión más actualizada de tu blog. Solo no olvides usar *git add* para agregar los archivos al Staging primero, y *git commit* para guardar los cambios antes de enviarlos a GitHub (de lo contrario no enviarás nada).

Después de completar este tutorial, deberías tener una estructura inicial bastante simple. Te sugiero pedirle a una IA que te ayude a hacerlo más bonito y adaptarlo como quieras. Lo importante, que es la estructura, ya está hecho.


## Recursos Útiles

Puedes consultar la [Documentación Oficial de Hugo](https://gohugo.io/) para aprender más. Encontrarás mucha información importante en la pestaña "Docs".

Un video que me ayudó mucho a empezar a aprender sobre Hugo fue este de **Fireship** (contenido en inglés):
{{< youtube 0RKpf3rK57I >}}


También recomiendo el video de **NetworkChuck** (contenido en inglés), donde enseña a crear un blog con Hugo, aplicar un tema y usar **Obsidian** para escribir los posts.
{{< youtube dnE7c0ELEH8 >}}


## Cierre

Como ya mencioné en el primer post que escribí para este blog, uno de mis objetivos es mejorar mi escritura de forma progresiva. Sé que este artículo está lejos del nivel que quiero que alcance, pero creo que ya es una introducción decente para quien busca aprender sobre Hugo. Te animo a leer la documentación oficial, ya que es allí donde encontrarás todo lo que necesitas. Si no sabes inglés, puedes usar herramientas como Google Translate, o mejor aún, pedirle a una IA que te explique la documentación traducida a tu idioma nativo.

Todavía sé muy poco sobre Hugo y estoy aprendiendo más conforme escribo estos posts. Si me dan ganas, quizás traiga más posts hablando sobre funcionalidades y cosas interesantes que podemos hacer con esta herramienta, como por ejemplo la lógica de la Table of Contents (ese menú de navegación por títulos de la página) que implementé en mi propio blog mientras escribía este post.