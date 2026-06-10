---
title: "Todo tiene un inicio"
slug: "todo-tiene-un-inicio"
date: 2026-05-14T10:00:00-03:00
draft: false
categories: ["Inicio"]
series: ["Todo Tiene un Inicio"]
description: "El comienzo de MitzIdeas."
translationKey: "primeiro-post"
---

¡Hola! Mi nombre es Davi Schmitz, tengo 21 años (intencionalmente publiqué este primer post el día de mi cumpleaños), soy un desarrollador Junior que actualmente trabaja con el framework Django y estudio Ciencias de la Computación.

<small>Me pareció necesario presentarme aquí porque iré actualizando la página "Sobre mí" con el tiempo, así que esta es una introducción que probablemente quedará desactualizada pronto.</small>

---

**MitzIdeas** es mi blog — mi espacio para registrar ideas, estudios y experiencias.

Este blog es un registro de mi vida: carrera, estudios, pasatiempos, libros que leo y todo lo que me parezca interesante compartir.

El objetivo principal es que, a través de este proyecto, pueda consolidar el conocimiento que obtengo en mis estudios, ya que aprendemos mucho al intentar enseñar o explicar a otra persona. También quiero desarrollar mi escritura, algo que deseo hacer desde hace tiempo. Además, como no veo este proyecto como algo a corto plazo, lo que escriba aquí servirá como un historial para que mi "yo del futuro" recuerde y comprenda toda la trayectoria recorrida. Mis errores, aciertos, ideas... todo quedará guardado.

Sobre mi escritura, sé que todavía escribo bastante mal. Con frecuencia cambio de tema y acabo volviendo a mi idea anterior. Esta característica es casi una firma personal. Probablemente usaré IAs para que me ayuden a corregir errores gramaticales y sugieran mejoras, pero garantizo algo: *NUNCA* permitiré que las IAs tomen la decisión final sobre cómo quedará mi texto. Por encima de todo, quiero imprimir mi identidad en cada publicación. Internet ya está lleno de textos superficiales generados por IA, algo que realmente no aguanto más.


---

Esta es la primera publicación de mi blog y no podía perder la oportunidad de contar un poco sobre cómo fue creado. Si eres del área de tecnología, probablemente reconocerás el aspecto de este blog de algún lugar. Sigo a Fabio Akita desde hace tiempo porque su contenido es esencial para quienes quieren aprender de verdad sobre programación, y utilicé básicamente las mismas herramientas que él para crear mi propio blog. Para ser sincero, no tenía idea de cómo usar estas herramientas y todavía estoy aprendiendo, pero como estamos en una época en la que podemos producir cosas geniales con la ayuda de LLMs, Claude AI fue de gran ayuda para dejar el blog con la apariencia que deseaba.

Como me conozco, es muy probable que realice aún más cambios en el futuro, así que decidí guardar aquí una captura de pantalla de cómo fue la primera versión de la página de inicio.

{{< figure src="MitzIdeasHome.png" title="La primera versión de la página de inicio de MitzIdeas (Mayo de 2026)." alt="Captura de pantalla de la página de inicio del blog" >}}

Sin embargo, lo que más interesa aquí es lo que hay detrás de la estructura, lo que permite que creemos publicaciones como queramos, de forma rápida y organizada. Para ello, intentaré dar una pequeña introducción sobre RSS, Hextra y Hugo, que es lo que está por detrás de todo esto. También explicaré sobre la internacionalización del blog, específicamente cómo traduzco la estructura y el contenido de las publicaciones.

### Entendiendo RSS

RSS es la sigla de *Really Simple Syndication*. De forma simplificada, es un archivo XML que lista tus publicaciones recientes, permitiendo que lectores de feed como Feedly o NetNewsWire sepan que has publicado algo sin necesidad de visitar tu sitio.

### Hugo

Hugo es un generador de sitios estáticos (SSG - Static Site Generator) escrito en Go. Cada parte de este blog es un archivo Markdown (.md) que Hugo procesa a través de las plantillas HTML que tengo en una carpeta llamada *layouts* y "escupe" un sitio estático (HTML/CSS/JS puro) en la carpeta *public*. Es extremadamente rápido (genera cientos de páginas en milisegundos) y seguro, ya que no tiene una base de datos ejecutándose en tiempo real en el servidor. Debido a que todo es estático, puedo hospedarlo en plataformas de forma gratuita, que era exactamente lo que buscaba: poder compartir mis ideas sin tener que pagar alojamiento.

### Hextra

Hextra es el tema o framework de diseño que utilizo sobre Hugo. Facilita muchas cosas, como la creación de la barra de búsqueda de publicaciones, la configuración de *Dark Mode* y *Light Mode* y la organización de las secciones. Lo personalicé para tener un aspecto más "clean" y minimalista, ya que la intención era que el contenido recibiera casi toda la atención del lector; siempre he tenido problemas con sitios o interfaces que muestran demasiada información en pantalla.

### Internacionalización y traducción

Habrás notado que el sitio y las publicaciones están traducidos a otros idiomas. Para ello, utilicé el *Multilingual Mode* de Hugo. ¿Recuerdas que comenté que cada parte del blog es un archivo Markdown? Guardo cada uno de ellos con la información del idioma en la extensión, como *.en.md* para el inglés. Ejemplos: *primeiro-post.en.md* para inglés o *primeiro-post.fr.md* para francés. Hugo crea árboles de directorios separados para cada idioma en la compilación final (/en/, /es/, etc.). Además, utilizo archivos en la carpeta *i18n/* o configuraciones en el *hugo.toml* para traducir botones como "Volver" o "Siguiente".

---
Montar toda esta estructura desde cero ya fue un gran desafío, así que finalizaré este primer post aquí para mantenerlo más simple. A medida que gane experiencia y fluidez con estas herramientas, los próximos textos serán cada vez más elaborados.