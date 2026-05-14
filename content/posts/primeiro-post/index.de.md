---
title: "Alles hat einen Anfang"
date: 2026-05-14T10:00:00-03:00
draft: false
categories: ["Anfang"]
series: []
description: "Der Beginn von MitzIdeas."
---

Hallo! Mein Name ist Davi Schmitz, ich bin 21 Jahre alt (ich habe diesen ersten Beitrag absichtlich an meinem Geburtstag veröffentlicht), Junior-Entwickler, der derzeit mit dem Django-Framework arbeitet, und Informatikstudent.

<small>Ich hielt es für notwendig, mich hier vorzustellen, da ich die „Über mich“-Seite im Laufe der Zeit aktualisieren werde. Diese Einleitung wird also wahrscheinlich bald veraltet sein.</small>

---

**MitzIdeas** ist mein Blog – mein Platz, um Ideen, Studien und Erfahrungen festzuhalten.

Dieser Blog ist ein Protokoll meines Lebens: Karriere, Studium, Hobbys, Bücher, die ich lese, und alles andere, was mir interessant erscheint, um es zu teilen.

Das Hauptziel dieses Projekts ist es, das Wissen, das ich durch mein Studium erwerbe, zu festigen, da man viel lernt, wenn man versucht, es jemand anderem beizubringen oder zu erklären. Außerdem möchte ich mein Schreiben verbessern, etwas, das ich schon lange tun wollte. Da ich dieses Projekt nicht als kurzfristig betrachte, sollen meine Texte hier als Historie für mein zukünftiges Ich dienen, um den gesamten Weg nachzuvollziehen. Meine Fehler, Erfolge, Ideen... alles wird gespeichert.

Was mein Schreiben betrifft, weiß ich, dass ich immer noch ziemlich schlecht schreibe. Oft wechsle ich das Thema und komme dann zu meiner vorherigen Idee zurück. Diese Eigenschaft ist fast schon meine Handschrift. Wahrscheinlich werde ich KIs einsetzen, um Grammatikfehler zu korrigieren und Verbesserungen vorzuschlagen, aber eines garantiere ich: Ich werde *NIEMALS* zulassen, dass die KI die endgültige Entscheidung darüber trifft, wie mein Text am Ende aussieht. Vor allem möchte ich meine Identität in jedem Post bewahren. Das Internet ist bereits voll von oberflächlichen, KI-generierten Texten, was ich ehrlich gesagt nicht mehr ertragen kann.


---

Dies ist der erste Beitrag in meinem Blog und ich konnte die Gelegenheit nicht verpassen, ein wenig darüber zu erzählen, wie er entstanden ist. Wenn du aus dem Tech-Bereich kommst, hast du das Design dieses Blogs wahrscheinlich schon irgendwo wiedererkannt. Ich folge Fabio Akita schon eine Weile, da seine Inhalte essenziell für jeden sind, der wirklich Programmieren lernen will, und ich habe im Grunde die gleichen Tools wie er benutzt, um meinen eigenen Blog zu erstellen. Um ehrlich zu sein, hatte ich keine Ahnung, wie man diese Tools benutzt, und ich lerne immer noch, aber da wir in einer Zeit leben, in der wir mit Hilfe von LLMs schon viele coole Dinge produzieren können, war Claude AI eine große Hilfe, um dem Blog das Aussehen zu geben, das ich mir gewünscht habe.

Da ich mich selbst kenne, werde ich in Zukunft wahrscheinlich noch mehr Änderungen vornehmen. Deshalb habe ich mich entschieden, hier einen Screenshot der ersten Version der Homepage des Blogs zu speichern.

{{< figure src="MitzIdeasHome.png" title="Die erste Version der MitzIdeas-Homepage (Mai 2026)." alt="Screenshot der Blog-Homepage" >}}

Was jedoch am meisten zählt, ist die Struktur dahinter, die es uns ermöglicht, Beiträge so zu erstellen, wie wir wollen, schnell und organisiert. Zu diesem Zweck werde ich versuchen, eine kurze Einführung in RSS, Hextra und Hugo zu geben. Ich werde auch die Übersetzung oder Internationalisierung des Blogs erklären, insbesondere wie ich die Blogstruktur und den Inhalt der Beiträge übersetze.

### RSS verstehen

RSS steht für *Really Simple Syndication*. Vereinfacht gesagt handelt es sich um eine XML-Datei, die deine neuesten Beiträge auflistet. So wissen Feed-Reader wie Feedly oder NetNewsWire, dass du etwas Neues veröffentlicht hast, ohne dass sie deine Website besuchen müssen.

### Hugo

Hugo ist ein statischer Website-Generator (SSG), geschrieben in Go. Jeder Teil dieses Blogs ist eine Markdown-Datei (.md), die Hugo durch die HTML-Templates in meinem Ordner *layouts* verarbeitet und eine statische Website (reines HTML/CSS/JS) in den Ordner *public* "ausspuckt". Er ist extrem schnell (erzeugt hunderte von Seiten in Millisekunden) und sicher, da keine Echtzeit-Datenbank auf dem Server läuft. Da alles statisch ist, kann ich die Seite kostenlos auf verschiedenen Plattformen hosten. Genau das habe ich gesucht: meine Ideen teilen zu können, ohne für das Hosting bezahlen zu müssen.

### Hextra

Hextra ist das Theme oder Design-Framework, das ich zusätzlich zu Hugo verwende. Es erleichtert viele Dinge, wie das Erstellen der Suchleiste für Beiträge, die Konfiguration von *Dark Mode* und *Light Mode* sowie die Organisation der Sektionen. Ich habe es angepasst, um einen "cleanen" und minimalistischen Look zu erzielen, da die Aufmerksamkeit des Lesers fast ausschließlich dem Inhalt gelten soll. Ich hatte schon immer Probleme mit Websites oder Oberflächen, die zu viele Informationen auf dem Bildschirm anzeigen.

### Internationalisierung und Übersetzung

Du hast sicher bemerkt, dass die Website und die Beiträge in andere Sprachen übersetzt sind. Dafür habe ich den *Multilingual Mode* von Hugo genutzt. Erinnerst du dich, dass ich erwähnt habe, dass jeder Teil des Blogs eine Markdown-Datei ist? Ich speichere jede davon mit der Sprachinformation in der Dateiendung, wie z. B. *.en.md* für Englisch. Beispiele: *primeiro-post.en.md* für Englisch oder *primeiro-post.fr.md* für Französisch. Hugo erstellt beim finalen Build separate Verzeichnisstrukturen für jede Sprache (/en/, /es/ usw.). Zusätzlich verwende ich Dateien im Ordner *i18n/* oder Einstellungen in der *hugo.toml*, um Schaltflächen wie „Zurück“ oder „Weiter“ zu übersetzen.

---
Diese gesamte Struktur von Grund auf neu aufzubauen, war bereits eine große Herausforderung. Deshalb werde ich diesen ersten Beitrag hier beenden, um ihn einfacher zu halten. Je mehr Erfahrung und Routine ich im Umgang mit diesen Werkzeugen sammle, desto ausführlicher werden die zukünftigen Texte sein.