# Research Database

An Obsidian-based research knowledge base for neuroimaging, MRI/QSM methods, literature review, research records, and manuscript drafting.

This repository is organized as a working research vault: it stores paper notes imported from Zotero, structured research records, reusable writing templates, daily notes, and active manuscript workspaces.

## Repository Structure

```text
.
├── !!!Start Here.md
├── Daily Notes/
├── Knowledge Summary/
│   ├── Research Records/
│   └── Words and Phrases/
├── Literature Notes/
├── MOC/
├── Supplementary FIles/
├── Templates/
│   ├── Paper Helper/
│   └── ZotLit/
└── Writing/
```

## Main Areas

### `!!!Start Here.md`

The vault entry point. It contains the active task query and the recommended workflow for daily notes, literature review, research records, and terminology notes.

### `Literature Notes/`

Imported and annotated literature notes, typically generated from Zotero using ZotLit. Notes include citation metadata, DOI/publication details, Zotero links, attachment links, abstracts, and annotation blocks.

### `Knowledge Summary/Research Records/`

Structured research records for projects, methods, experiments, and research ideas. The default template includes:

- aims
- keywords and tags
- research design
- runtime configuration
- data
- methods
- results
- discussion

### `Knowledge Summary/Words and Phrases/`

Concept notes and terminology summaries, organized by field and topic. Current examples include MRI/QSM reconstruction and toolbox notes.

### `Writing/`

Manuscript workspaces. Each writing project has a dashboard note and section notes for guidance, introduction, background, related works, methods, results/discussion, abstract/title, and final checks.

### `Templates/`

Reusable Obsidian templates for:

- daily notes
- research notes
- terminology explanations
- paper drafting workflows
- ZotLit literature-note formatting

## Recommended Obsidian Setup

This vault is designed to work best in Obsidian with the following community plugins:

- Templater
- Tasks
- ZotLit
- Omnisearch
- Table Editor
- Excalidraw

The `.obsidian/` directory is included so plugin and vault configuration can be reused when opening the repository as an Obsidian vault.

## Workflow

### Daily Work

1. Open `!!!Start Here.md`.
2. Create or review the daily note in `Daily Notes/`.
3. Check unfinished tasks using the Tasks query.
4. Add new TODOs, research ideas, or reading notes as needed.

### Literature Review

1. Annotate papers in Zotero.
2. Use the highlight color convention documented in `!!!Start Here.md`.
3. Create or update the Obsidian literature note through ZotLit.
4. Synthesize important findings into `Knowledge Summary/Research Records/` when they become reusable research knowledge.

### Research Records

Use the `Research Note` template through Templater. New research records are placed under:

```text
Knowledge Summary/Research Records/
```

### Manuscript Drafting

Use the `Paper Helper` template through Templater. It creates a writing dashboard and section notes under:

```text
Writing/<Paper Title>/
```

The default drafting order is:

1. Methods
2. Results + Discussion
3. Introduction
4. Abstract + Title
5. Final consistency check
6. Revision based on reviewer/editor comments

## Naming and Organization

- Literature notes generally use citation-key-style filenames.
- Research records use descriptive project or topic titles.
- Writing projects are grouped in their own folder under `Writing/`.
- Terminology notes are nested by domain, for example `MRI/QSM/Reconstruction/`.

## Notes

- Some links use Obsidian wiki-link syntax such as `[[Biological Reviews]]`.
- Zotero attachment links may point to local files and may not resolve on other machines.
- This repository is primarily a knowledge-management vault, not a software package.

