# MitzIdeas Translation Invariants

## Content model

The project is a Hugo blog with five content roots:

```text
content/pt-br/
content/en/
content/es/
content/fr/
content/de/
```

Posts are page bundles. Each translated post normally has this shape:

```text
content/<language>/posts/<series-directory>/<post-directory>/index.md
```

Keep existing directory names and bundled asset filenames unless the user explicitly asks to rename and update every reference. Do not translate paths as a side effect of prose translation.

## Front matter

The current post schema is:

```yaml
---
title: ""
slug: ""
date: 2026-05-14T10:00:00-03:00
draft: false
categories: [""]
series: [""]
description: ""
translationKey: ""
---
```

| Item | Translation rule |
| --- | --- |
| Keys and YAML delimiters | Preserve exactly. |
| `title`, `description` | Translate naturally for the target reader. |
| `slug` | Localize to a concise URL-safe slug. Do not copy the Portuguese slug unless it is a proper name or the user asks. |
| `categories`, `series` | Translate and keep consistent with existing target-language taxonomy. |
| `date` | Preserve exactly across versions of the same post. |
| `draft` | Preserve exactly. |
| `translationKey` | Preserve exactly. It links language versions in Hugo and must never be translated. |

## Protected syntax and data

Preserve these byte-for-byte unless the user explicitly asks to change their behavior:

- Hugo delimiters and expressions: `{{ ... }}`, `{{< ... >}}`, `{{% ... %}}`.
- Shortcode names and parameter names, such as `figure`, `src`, `title`, and `alt`.
- Asset paths in shortcode `src` values, including image filenames and directories.
- URLs and link destinations. Translate the visible Markdown link label, not the target in parentheses.
- HTML element and attribute names; preserve `href`, `src`, `class`, `id`, and attribute values that are URLs, paths, identifiers, or code. Translate human-facing `alt`, `title`, and text-node content.
- Markdown structure: heading markers, emphasis delimiters, list markers, table separators, blockquote markers, horizontal rules, and fenced-code delimiters.
- Inline code, fenced code, commands, flags, file paths, configuration keys, template syntax, and literal command output.
- Proper names and technical identifiers, including `MitzIdeas`, Hugo, GitHub, GitHub Actions, Hextra, Go Templates, `hugo.toml`, `hugo server`, `translationKey`, and language directory codes.

## Preservation audit

Before completing a translation, verify all of the following:

1. The source and target front matter have the same keys in the same structure.
2. `date`, `draft`, and `translationKey` match exactly.
3. Hugo shortcode names, parameters, counts, and asset `src` values match exactly; only reader-facing `title`/`alt` prose may differ.
4. Every URL, Markdown link destination, file path, inline-code span, and fenced-code block remains unchanged.
5. Heading hierarchy, list/table structure, raw HTML structure, and Markdown delimiters are unchanged.
6. The target has reader-facing translation for title, description, taxonomy labels, headings, visible link text, figure copy, and prose.
7. The target reads as natural writing in the requested language and retains the original stance, facts, dates, and level of certainty.

## Localized taxonomy already in use

| Portuguese (Brazil) | English | Spanish | French | German |
| --- | --- | --- | --- |
| `Início` | `Beginning` | `Inicio` | `Début` | `Anfang` |
| `Tudo tem um início` | `Everything has a beginning` | `Todo Tiene un Inicio` | `Tout a un début` | `Alles hat einen Anfang` |

Use these existing forms for the current series. For a new taxonomy, choose a natural term for each language and reuse it consistently in every related post.
