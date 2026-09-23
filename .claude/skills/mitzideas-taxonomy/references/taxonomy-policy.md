# MitzIdeas Taxonomy Policy

## The difference

| Field | Purpose | Mental model | Typical count |
| --- | --- | --- | --- |
| `categories` | Reusable subjects that help a reader discover related posts across the blog. | Shelf in a library. | One primary; up to three when all are substantial. |
| `series` | A named, coherent sequence of posts about one continuing project, book, course, journey, or tutorial. | A playlist or chapter sequence. | None or one in normal use. |
| `tags` | Configured in Hugo, but not yet governed or surfaced by the home UI. | Not in use yet. | Leave empty unless the user defines a tag policy. |

A category answers **“what is this post about?”** A series answers **“which continuing story or study path does this post belong to?”** They are complementary, not substitutes.

## Existing values and their scope

| Field | Current value | Scope |
| --- | --- | --- |
| Category | `Início` | Reserved for the initial MitzIdeas posts that explain the creation of the blog. This is not a general beginner or introductory label. |
| Series | `Tudo tem um início` | The sequence documenting how MitzIdeas was created with Hugo. |

The current localized forms remain the source of truth for translations:

| PT-BR | EN | ES | FR | DE |
| --- | --- | --- | --- | --- |
| Category: `Início` | `Beginning` | `Inicio` | `Début` | `Anfang` |
| Series: `Tudo tem um início` | `Everything has a beginning` | `Todo Tiene un Inicio` | `Tout a un début` | `Alles hat einen Anfang` |

## Future category vocabulary

These are approved examples of durable categories, not a requirement to apply all of them:

- `Front-End`: interfaces, browser behavior, CSS, accessibility, client-side development, and UI engineering.
- `Back-End`: servers, APIs, databases, Django, architecture, authentication, and server-side development.
- `IA`: artificial intelligence, language models, AI tools, machine learning, and critical reflections on their use.
- `Redes`: networking, protocols, internet infrastructure, distributed communication, and network studies.

Other useful categories may emerge from sustained interests, hobbies, career, books, studies, or personal documentation. Add one only when it will remain useful beyond a single post. Prefer a specific, stable subject over a vague mood or format label.

## Series policy

Use a series when several posts share a clear promise and readers benefit from seeing them together. The series title should identify that promise, not merely repeat a category.

Example:

```yaml
categories: ["Algoritmos"]
series: ["Entendendo Algoritmos — Aditya Y. Bhargava"]
```

Here, `Algoritmos` is the reusable subject. `Entendendo Algoritmos — Aditya Y. Bhargava` is the particular reading-and-learning sequence. A future algorithms post unrelated to that book may use `Algoritmos` without that series.

Do not add a series merely because a post has “Parte 1” in its title. Confirm that the author intends an ongoing sequence. Conversely, a series may start with one post if its continuing intent is clear.

## Naming and multilingual consistency

- Keep a canonical PT-BR name for every category and series, then use a deliberate equivalent in each language version.
- Reuse the exact existing spelling within a language. Preserve capitalization and accents consistently.
- Category and series values are reader-facing and should be translated; `translationKey` is structural and must remain identical.
- Do not translate directory names, asset paths, URLs, code, or Hugo syntax as part of taxonomy work.

## Before finalizing

- Does the primary category describe the post's enduring main subject?
- Would each extra category help a reader find this post, rather than merely record a mention?
- Does the series name identify a specific continuing sequence?
- Is `Início` being used only for the original blog-creation posts?
- Is the value an exact existing label or a justified, durable new one?
- Are the labels translated consistently in all language versions?
