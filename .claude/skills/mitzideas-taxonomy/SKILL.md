---
name: mitzideas-taxonomy
description: Classify MitzIdeas posts with coherent Hugo categories and series. Use when drafting, creating, editing, or reviewing a post's `categories` or `series` front matter, including translations and requests to organize the blog.
---

# MitzIdeas Taxonomy

Classify a post so readers can find its subject without losing the story it belongs to. Categories describe recurring subject areas; series describe a specific continuing editorial journey. Read [taxonomy policy](references/taxonomy-policy.md) before choosing values.

## Workflow

1. Read the complete post and identify its primary subject, secondary subjects that receive substantial treatment, and whether it belongs to an ongoing narrative, study path, book, project, or tutorial sequence.
2. Choose one primary category. Add a second or third only when each is genuinely useful for discovery. Do not apply a label because a term is mentioned in passing.
3. Add a series only if the post belongs to a named sequence. A post may have categories and no series.
4. Reuse exact existing labels whenever they fit. Create a new category only when it is a durable subject likely to cover more than one post; create a new series only when there is a real shared thread across posts.
5. Preserve the front matter array structure. In translations, localize reader-facing category and series labels consistently while preserving `translationKey` and all other protected translation invariants.
6. State the selected category or categories and series in the completion summary, along with a one-sentence rationale if a new label was introduced.

## Decision rules

- Treat `Início` as a historical, limited category for the initial posts that explain the creation of MitzIdeas. Do not reuse it for ordinary future introductions, first entries of unrelated series, or generic beginner content.
- Treat `Tudo tem um início` as the specific series about building this Hugo blog. Do not use it for posts that only mention Hugo or personal beginnings without belonging to that narrative.
- Use broad topic categories such as `Front-End`, `Back-End`, `IA`, and `Redes` for the enduring subject of a post. They can coexist when the post substantively teaches or reflects on both topics.
- A book-study sequence is a series, not a category. For example, posts following the same book belong to `Entendendo Algoritmos — Aditya Y. Bhargava`; give each post one or more broad subject categories that describe its actual lesson.
- The configured Hugo `tags` taxonomy has no current project convention or home-page filter. Do not add `tags` as a synonym for `categories` unless the user explicitly introduces a tag policy.
- Do not create near-duplicates that differ only by capitalization, singular/plural, accents, language mixing, or wording. Check existing content first.
- Do not force a series onto a standalone reflection, hobby, career update, or note. Categories alone are valid.
