<%*
const sectionName = "02 Introduction";
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
title: "<% paperTitle %> - 02 Introduction"
type: writing-section
paper_title: "[[<% safePaperTitle %>|<% paperTitle %>]]"
paper_title_text: "<% paperTitle %>"
section: "02 Introduction"
created: <% tp.date.now("YYYY-MM-DD") %>
---
Parent paper: [[<% safePaperTitle %>|<% paperTitle %>]]

## Introduction

### Purpose

Use the CARS model: establish the territory, establish the niche, occupy the niche.

For biomedical MRI and computational neuroimaging papers, the Introduction usually connects three layers:

1. Disease or clinical problem.
2. MRI/neuroimaging opportunity and limitation.
3. Methodological gap and present contribution.

### Move 1: Establish the Territory

Goal:

- Show that the research area is important, current, or scientifically relevant.
- Give the reader enough context to understand the study.
- Establish credibility through relevant literature.
- For neurology journals, start with disease burden, diagnostic challenge, biomarker need, or clinical workflow.
- For computer science or medical image computing venues, start with the imaging analysis problem and why it is technically difficult.

Draft:

> **Broad statement about the field/problem and why it matters.**
> **More specific background about the topic.**
> **Brief synthesis of relevant current knowledge with citations.**

Biomedical MRI version:

> **Disease or vascular/neurodegenerative process** is associated with **clinical outcome or diagnostic problem**.
> MRI provides **non-invasive imaging information**, but **current limitation**s restrict its use for target clinical or computational tasks.

### Move 2: Establish the Niche

Goal:

- Identify the research gap, problem, limitation, controversy, or needed extension.
- Contrast previous studies with the present need.
- State whether the gap is clinical, biological, imaging-technical, computational, or validation-related.

Draft:

> Previous studies have shown that **known finding**. However, **gap/limitation/uncertainty**.
> Although **existing work**, little is known about the **specific gap**.
> Prior research has focused on **X**, whereas **Y** remains unclear.

Common MRI/computational gaps:

> Existing MRI biomarkers can quantify **biological process**es, but their relationship with **clinical phenotype/outcome** remains unclear.
> Deep learning methods have been applied to the **task**, but external validation across **scanner/cohort/site** is limited.
> Classical image analysis methods, such as filter/segmentation/reconstruction approaches, are sensitive to **noise, artifacts, partial volume, bifurcations, lesion heterogeneity, or domain shift**.
> Current studies often report **technical metric**s, whereas evidence for **clinical relevance or neurological interpretation** remains insufficient.

### Move 3: Occupy the Niche

Goal:

- State the objective, research question, hypothesis, and scope of the present study.
- Make the contribution explicit for both technical and clinical readers.

Draft:

> To address this gap, this study aimed to **objective**.
> We investigated **scope/population/materials/variables** using **approach**.
> Specifically, we asked whether **research question/hypothesis**.

Contribution statement:

> We developed/evaluated **method/model/biomarker** for **MRI task** and assessed its performance using **internal validation, external validation, clinical association, or statistical analysis**.
> We hypothesized that **MRI-derived measure or model output** would be associated with **disease status, subtype, pathology, cognitive score, vascular burden, or clinical outcome**.

### Rules

- Move from general to specific.
- Use citations by synthesis, not by listing one paper after another.
- Use gap signals: however, but, although, whereas, nevertheless, little is known.
- Keep the final paragraph focused on the present study.
- Avoid overstating AI as a replacement for clinical judgment or reference-standard imaging.
- Name the target task precisely: segmentation, reconstruction, synthesis, classification, prediction, quantification, or association.

### Tense

- Present tense: general truths, current knowledge, established facts.
- Past tense: specific findings from specific studies.
- Present perfect: research attention or unresolved trends continuing into the present.

### Checklist

- [ ] Starts broad enough for the target reader.
- [ ] Narrows toward the specific research problem.
- [ ] Identifies a clear gap.
- [ ] States objective, question, hypothesis, and scope.
- [ ] Ends by positioning the present study.
