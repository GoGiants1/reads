# Repository Guidelines

This repository stores paper reading notes intended to be browsed on GitHub.

## Structure

Use this layout for each paper:

```text
papers/{year}/{arxiv-id}-{short-slug}/README.md
papers/{year}/{arxiv-id}-{short-slug}/metadata.yaml
```

Example:

```text
papers/2026/2603.21786-universal-normal-embedding/
├── README.md
└── metadata.yaml
```

Keep the main note in `README.md` so GitHub renders it automatically when opening the paper directory.

## Naming Rules

- Use ASCII-only paths.
- Use lowercase slugs with hyphens.
- Put the arXiv ID first when available.
- Do not include arXiv version suffixes in directory names. Use `2603.21786`, not `2603.21786v1`.
- Record the version in `metadata.yaml`.
- Prefer concise slugs: `2603.21786-universal-normal-embedding`.
- For non-arXiv papers, use `{year}/{first-author-year-short-title}` unless a stable identifier exists.

## Paper Note Content

Start from `templates/paper-note.md` when creating a new note.

For Korean notes, headings may be Korean or English, but keep file and directory names in ASCII.

Each paper note should include:

- bibliographic metadata near the top
- one-line summary
- key idea
- method
- experiments
- results
- discussion or interpretation
- limitations
- references / source links

If a discussion becomes long, keep the main synthesis in `README.md` and optionally add sibling files such as:

```text
discussion.md
experiments.md
ideas.md
assets/
```

Only create these when they are useful.

## Metadata

Every paper directory should include `metadata.yaml` with this shape:

```yaml
title: "Paper Title"
arxiv_id: "2603.21786"
version: "v1"
year: 2026
venue: "CVPR 2026"
authors:
  - Author One
  - Author Two
tags:
  - representation-learning
  - diffusion
read_date: "2026-04-21"
language: ko
status: summarized
links:
  arxiv: "https://arxiv.org/abs/2603.21786v1"
```

If a field is unknown, use an empty string or omit it rather than guessing.

## Root Index

Update root `README.md` whenever adding a paper.

Add one row to the `Papers` table:

```markdown
| 2026-04-21 | Paper Title | topic-a, topic-b | [note](papers/2026/id-slug/) |
```

Keep links directory-based so GitHub opens and renders the paper folder README.

## Topics

`topics/` is for manual topic-level indexes after enough notes accumulate. Do not overbuild it for a single paper.

When adding topic pages later, keep them simple lists of links grouped by theme.

## Git Hygiene

- Do not commit `.codex`.
- Keep `.gitignore` updated for local or tool-generated files.
- Stage only intended files.
- Commit messages should be short and descriptive, for example:
  - `Add UNE paper note`
  - `Add diffusion topic index`
  - `Restructure paper notes`

## Public Repository Safety

Assume everything committed here is public.

Before committing, check:

- no private notes, credentials, API keys, or local paths that should not be public
- no downloaded PDFs unless explicitly intended
- no generated temporary files
- source links are valid and useful

## Preferred Workflow

For a new arXiv paper:

1. Extract the bare arXiv ID without version suffix.
2. Determine year, title, short slug, authors, venue, and tags.
3. Create `papers/{year}/{arxiv-id}-{slug}/`.
4. Write the main note to `README.md`.
5. Write `metadata.yaml`.
6. Update root `README.md`.
7. Run `git status --short`.
8. Commit and push.
