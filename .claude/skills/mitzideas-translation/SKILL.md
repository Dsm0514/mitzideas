---
name: mitzideas-translation
description: Translate or localize MitzIdeas Markdown posts and interface copy between Portuguese (Brazil), English, Spanish, French, and German. Use when creating or revising a translation for this Hugo blog, especially when preserving front matter, translation keys, Hugo syntax, assets, URLs, code, and the author's voice.
---

# MitzIdeas Translation

Translate for a reader of the target language, not word for word. Preserve the author's point of view, certainty, humor, informality, deliberate rhythm, and technical accuracy. Adapt idioms, examples, punctuation, and sentence structure when a literal rendering would sound unnatural.

Before translating a post, read [project invariants](references/project-invariants.md). Apply those rules to every edit.

## Workflow

1. Identify the source language, target language, source file, and destination file. If the target language or destination is ambiguous, ask before creating or overwriting a file.
2. Read the entire source post, including its front matter, Markdown, raw HTML, shortcodes, link destinations, code, and image references.
3. Translate all reader-facing prose and translatable metadata. Localize the title, description, slug, categories, series, visible link labels, figure `title` and `alt` values, headings, and prose.
4. Keep protected structure exact. Do not translate or alter front matter keys, `translationKey`, date, draft state, Hugo/Go syntax, shortcode names, asset paths, URLs, IDs, classes, commands, code blocks, inline code, or Markdown delimiters.
5. Run the preservation audit in the reference before finalizing. Correct any changed protected value.
6. Report the target file and briefly flag only genuine unresolved translation choices. Do not add explanatory text, citations, or new claims to the post.

## Writing decisions

- Prefer an idiomatic target-language sentence over a literal cognate or Portuguese word order.
- Keep named technologies, product names, project name `MitzIdeas`, commands, file names, CLI flags, repository names, taxonomies, and programming terms in the form that preserves their technical meaning. Translate surrounding explanation when it helps.
- Translate categories and series consistently with the existing target-language taxonomy. Search the corresponding target-language content before inventing a new rendering.
- Localize a post `slug` to a concise, URL-safe version of its localized title. It is reader-facing metadata, unlike `translationKey`.
- Preserve intentional personal opinions and uncertainty. Do not make the author sound more formal, more certain, more enthusiastic, or more generic than the source.
- Keep code examples and command output verbatim. If prose appears inside a code block, do not translate it unless the user explicitly asks for an adapted code sample.
- When a phrase has no clean equivalent, preserve the intended effect rather than its individual words. If a choice could change meaning, ask a focused question instead of guessing.

## Humanizer

If the user explicitly asks to humanize generated or translated prose, use the project Humanizer skill after the translation is structurally correct. Restrict that pass to reader-facing prose. It must not change facts, the author's views, front matter, syntax, code, URLs, asset paths, or the target language.
