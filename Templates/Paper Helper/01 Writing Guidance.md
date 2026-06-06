<%*
const sectionName = "01 Writing Guidance";
const currentFolder = tp.file.folder(true) || "";
const folderParts = currentFolder.split("/");
const defaultPaperTitle = folderParts[0] === "Writing" && folderParts[1] ? folderParts[1] : "";
const paperTitle = await tp.system.prompt("Paper title", defaultPaperTitle);

if (!paperTitle) {
  new Notice("Paper title is required.");
  return;
}

const safePaperTitle = paperTitle
  .replace(/[\\/:*?"<>|#^[\]]/g, "")
  .replace(/\s+/g, " ")
  .trim();

const baseFolder = `Writing/${safePaperTitle}`;

async function ensureFolder(path) {
  const parts = path.split("/");
  let current = "";
  for (const part of parts) {
    current = current ? `${current}/${part}` : part;
    if (!app.vault.getAbstractFileByPath(current)) {
      await app.vault.createFolder(current);
    }
  }
}

await ensureFolder(baseFolder);

let targetPath = `${baseFolder}/${safePaperTitle} - ${sectionName}`;
if (app.vault.getAbstractFileByPath(`${targetPath}.md`)) {
  targetPath = `${targetPath} ${tp.date.now("YYYYMMDD-HHmmss")}`;
}

await tp.file.move(targetPath);
-%>
---
title: "<% paperTitle %> - 01 Writing Guidance"
type: writing-section
paper_title: "[[<% safePaperTitle %>|<% paperTitle %>]]"
paper_title_text: "<% paperTitle %>"
section: "01 Writing Guidance"
created: <% tp.date.now("YYYY-MM-DD") %>
---
Parent paper: [[<% safePaperTitle %>|<% paperTitle %>]]

## Writing Guidance

### Core Message

One-sentence summary:

> This paper argues that ...

Main contribution:

Why this paper matters:

### Target Audience

What does the audience already know?

What does the audience need to be convinced of?

What questions will the audience likely ask?

### Research Problem

Background:

Gap in existing literature:

Problem statement:

> The key problem addressed in this paper is ...

### Research Question and Hypothesis

Research question:

> This paper asks: ...

Hypothesis / central claim:

> I argue that ...

Sub-claims:

1. 
2. 
3. 

### Biomedical MRI Paper Profile

Primary audience:

- Clinical neurology
- Neuroradiology
- Biomedical engineering
- Computer vision / medical image computing
- MRI physics / technology

Paper style:

- Clinical neuroimaging study
- MRI biomarker study
- Deep learning method paper
- Computational image analysis paper
- Research letter / brief report
- Technical note

Disease and clinical context:

- Disease: Alzheimer disease / other neurodegenerative disease / cerebral small vessel disease / stroke / vascular disorder
- Clinical pain point: cost / radiation / accessibility / diagnostic delay / subjective visual rating / limited quantification / poor generalization
- Intended clinical use: diagnosis / prognosis / screening / triage / monitoring / subtype identification / mechanistic interpretation

Technical contribution:

- Input modality: structural MRI / DWI / QSM / MRA / SWI / FLAIR / PET / multimodal
- Output: segmentation / reconstruction / synthesis / classification / regression / biomarker quantification / association analysis
- Innovation: model architecture / prior or regularization / preprocessing pipeline / external validation / clinical translation / interpretability

### Paper Logic

```text
Clinical or biological problem
→ Imaging or computational limitation
→ Research gap
→ Proposed MRI analysis method
→ Validation evidence
→ Technical result
→ Clinical or neurological interpretation
→ Contribution
```

### Working Rules

- Keep the central claim visible while drafting every section.
- Make every major section answer one part of the research question.
- Reuse the same key terms across title, abstract, introduction, results, and discussion.
- Do not add claims that cannot be supported by data or citations.
- Mark's speculation is clear.
- Separate technical performance from clinical usefulness.
- For deep learning papers, state data source, labels, training/testing split, external validation, metrics, and failure modes.
- For clinical MRI papers, state cohort, inclusion/exclusion criteria, scanner/acquisition, imaging biomarker definition, statistical model, covariates, and clinical interpretation.
- Avoid claiming clinical deployment unless the study design supports it.
- Use cautious translational language: "may support", "could help", "shows potential", "requires further validation".
