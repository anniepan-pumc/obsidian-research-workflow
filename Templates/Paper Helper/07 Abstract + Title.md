<%*
const sectionName = "07 Abstract + Title";
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
title: "<% paperTitle %> - 07 Abstract + Title"
type: writing-section
paper_title: "[[<% safePaperTitle %>|<% paperTitle %>]]"
paper_title_text: "<% paperTitle %>"
section: "07 Abstract + Title"
created: <% tp.date.now("YYYY-MM-DD") %>
---
Parent paper: [[<% safePaperTitle %>|<% paperTitle %>]]

## Abstract + Title

### Abstract Purpose

The abstract is a short, standalone mini-version of the paper. Write or revise it after the manuscript is complete.

### Clinical Radiology / Neurology Structured Abstract

Objectives:

> **Clinical or imaging problem** is important because **reason**. This study aimed to **objective**.

Materials and methods:

> We included **cohort/dataset size and source** with **disease/control groups**. MRI was acquired using **sequence/modality**. **Model/biomarker/statistical method** was used to **task**, and performance/association was assessed using **metrics/statistics**.

Results:

> **Main numerical result**. **Secondary result or external validation result**. **Clinical association or diagnostic performance**.

Conclusions:

> **Main conclusion**. These findings may support **clinical or technical implication**, pending **needed validation if relevant**.

### Computer Science / Medical Image Computing Abstract

Problem:

> **MRI analysis task** remains challenging because **technical limitation**.

Method:

> We propose **method/model/prior** for **task**, using **key technical idea**.

Experiments:

> We evaluated the method on **dataset/cohort** against **baselines** using **metrics**.

Results:

> The proposed method achieved **primary result** and improved **metric/task** compared with **baseline**.

Impact:

> The results indicate that **technical contribution** may help **clinical or biological application**.

### Key Points / Clinical Relevance

Question:

> **Clinical or technical question**.

Findings:

> **Main finding with strongest evidence**.

Clinical relevance:

> **How the method/biomarker could help neurologists, neuroradiologists, or MRI researchers**.

### Abstract Rules

- Match the abstract to the title and manuscript.
- Focus on results related to the problem, purpose, and conclusion.
- Include specific details or data when useful.
- Avoid references, table/figure references, unfamiliar terms, excessive jargon, and unsupported conclusions.
- Do not include information absent from the main paper.
- For clinical journals, include cohort size, study design, MRI modality, primary statistics, and clinical relevance.
- For CS venues, include the technical gap, proposed method, baselines, datasets, metrics, and main improvement.
- For MRI synthesis or AI biomarker papers, state whether results are internal, external, retrospective, prospective, or multi-center.

### Abstract Tense

- Background/problem: present tense.
- Methods: past tense.
- Results: past tense.
- Recommendation/implication: present tense.

### Title Template

Main title:

> **Specific topic/finding**: **specific method, population, mechanism, or contribution**

Running title:

> **Short version of title**

### Title Rules

- Clear, complete, concise, and unambiguous.
- About 10-15 words or 100-150 characters unless the journal specifies otherwise.
- Avoid abbreviations and unnecessary articles at the beginning.
- Use a colon when it helps separate the broad topic from the specific focus.
- Make sure title, abstract, and manuscript correspond.
- For clinical MRI journals, foreground disease, MRI modality, and clinical/biomarker finding.
- For CS venues, foreground the method, task, and technical contribution.

### Example Title Patterns

- **MRI modality/biomarker** demonstrates **disease-relevant pattern** in **population**.
- **Deep learning method** for **MRI task** in **disease/application**.
- **Method/prior/model** for **vessel/lesion/biomarker reconstruction or quantification**.
- **Association between MRI feature** and **clinical outcome or disease phenotype**.

### Checklist

- [ ] Abstract states problem, purpose, methods, results, and conclusion.
- [ ] Abstract is understandable without reading the paper.
- [ ] Abstract contains no unsupported information.
- [ ] Title states the main topic.
- [ ] Title is specific enough to separate this paper from similar work.
- [ ] Running title is prepared if needed.
- [ ] Clinical relevance or technical contribution is visible from the title and abstract.
