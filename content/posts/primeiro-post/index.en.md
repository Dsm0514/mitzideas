---
title: "Everything has a beginning"
date: 2026-05-14T10:00:00-03:00
draft: false
categories: ["Beginning"]
series: []
description: "The start of MitzIdeas."
---

Hello! My name is Davi Schmitz, I'm 21 years old (I intentionally published this first post on my birthday), I'm a Junior Developer currently working with the Django framework and a Computer Science student.

<small>I felt it was necessary to introduce myself here because I'll be updating the "About" page over time, so this introduction will likely become outdated soon.</small>

---

**MitzIdeas** is my blog — my space to record ideas, studies, and experiences.

This blog is a record of my life: career, studies, hobbies, the books I read, and everything else I find interesting to share.

The main goal is that, through this project, I can consolidate the knowledge I gain from my studies, as we learn a lot by trying to teach or explain things to someone else. I also want to develop my writing, something I've wanted to do for a long time. Furthermore, since I don't see this as a short-term project, what I write here should serve as a history for my future self to remember and understand the entire journey. My mistakes, successes, ideas... everything will be saved.

Regarding my writing, I know I'm still not very good at it. I often change subjects and end up looping back to my previous idea. This trait is almost a signature of mine. I will probably use AI to help correct grammatical errors, suggest improvements, and other things to help me become a better writer. However, I guarantee one thing will *NEVER* happen: I will never let AI have the final say on how my text ends up. Above all, I want to put my identity into every post. The Internet is already full of shallow AI-generated content, which I honestly can't stand anymore.


---

This is the first post on my blog, and I couldn't miss the opportunity to talk a bit about how it was created. If you are from the tech field, you might recognize the look of this blog from somewhere. I've been following Fabio Akita for a while because his content is essential for anyone who wants to truly learn programming, and I used basically the same tools he uses to create my own blog. To be honest, I had no idea how to use these tools and I'm still learning, but since we live in an era where we can produce great things with the help of LLMs, Claude AI was a huge help in getting this blog to look exactly how I wanted.

Knowing myself, it's very likely I'll make even more changes in the future, so I decided to keep a screenshot here of what the first version of the blog's homepage looked like.

{{< figure src="MitzIdeasHome.png" title="The first version of the MitzIdeas homepage (May 2026)." alt="Screenshot of the blog homepage" >}}

However, what matters most is what's behind the structure, which allows us to create posts however we want, quickly, and keeping everything organized. To that end, I'll try to give a brief introduction to RSS, Hextra, and Hugo, which are the engines behind all of this. I will also explain the translation or internationalization of the blog—specifically, how I translate the blog structure and the post content.

### Understanding RSS

RSS stands for *Really Simple Syndication*. In simple terms, it's an XML file that lists your recent posts, allowing feed readers like Feedly or NetNewsWire to know when you've posted something without them needing to visit your site.

### Hugo

Hugo is a static site generator (SSG) written in Go. Every part of this blog is a Markdown (.md) file that Hugo processes through the HTML templates I have in a folder called *layouts* and "spits out" a static site (pure HTML/CSS/JS) into the *public* folder. It's extremely fast (generating hundreds of pages in milliseconds) and secure, as it has no real-time database running on the server. Because everything is static, I can host it on platforms for free, which was exactly what I was looking for: being able to share my ideas without having to pay for hosting.

### Hextra

Hextra is the theme or design framework I use on top of Hugo. It facilitates many things, such as creating the post search bar, configuring *Dark Mode* and *Light Mode*, and organizing sections. I customized it to have a cleaner, more minimalist look, as the intention was for the content to receive almost all of the reader's attention; I've always had issues with websites or interfaces that show too much information on the screen.

### Internationalization and translation

You may have noticed that the site and posts are translated into other languages. For this, I used Hugo's *Multilingual Mode*. Remember I mentioned that every part of the blog is a Markdown file? I save each one with the language information in the extension, such as *.en.md* for English. Examples: *primeiro-post.en.md* for English or *primeiro-post.fr.md* for French. Hugo creates separate directory trees for each language in the final build (/en/, /es/, etc.). Additionally, I use files in the *i18n/* folder or settings in *hugo.toml* to translate buttons like "Back" or "Next".

---
Setting up this whole structure from scratch was already a big challenge, so I'll wrap up this first post here to keep it simple. As I gain more experience and fluency with these tools, future posts will become increasingly elaborate.