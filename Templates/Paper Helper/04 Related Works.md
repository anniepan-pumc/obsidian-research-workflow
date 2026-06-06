<%*
const sectionName = "04 Related Works";
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
title: "<% paperTitle %> - 04 Related Works"
type: writing-section
paper_title: "[[<% safePaperTitle %>|<% paperTitle %>]]"
paper_title_text: "<% paperTitle %>"
section: "04 Related Works"
created: <% tp.date.now("YYYY-MM-DD") %>
---
Parent paper: [[<% safePaperTitle %>|<% paperTitle %>]]

## Related Works

### Purpose

The Related Works section positions the study in relation to existing research and identifies the gap that your paper addresses.

For computer science venues, Related Works may be a full independent section. For neurology or radiology journals, it is often folded into the Introduction.

### Literature Map

Clinical or biological literature:

- Key studies:
- What they show:
- Limitation:

MRI biomarker literature:

- Key studies:
- What they show:
- Limitation:

Classical computational / image processing methods:

- Key studies:
- What they show:
- Limitation:

Deep learning or machine learning methods:

- Key studies:
- Architecture or task:
- Dataset and validation:
- Limitation:

Clinical translation and validation:

- External validation:
- Multi-center/scanner evidence:
- Reader study or visual assessment:
- Clinical outcome association:
- Limitation:

### Synthesis Template

> Prior studies on the **topic** have mainly focused on **focus**. These studies show the **main finding**. However, they often **limitation/gap**.

> A second line of work examines the **topic**. This work contributes, but it remains unclear whether it addresses the **unresolved issue**.

> Together, these studies suggest **synthesis**. The present study extends this work with **your contribution**.

Computational MRI version:

> Classical methods, such as filter/model/optimization method, provide an **advantage**, but are limited by **noise/artifacts/scale variation/manual parameters/weak anatomical prior**.
> Deep learning methods improve task performance, yet many studies lack external validation, interpretability, uncertainty analysis, or linkage to clinical outcomes.
> Our work addresses this gap by **technical innovation** and evaluates it in a **dataset/cohort/clinical setting**.

### Rules

- Organize by theme, debate, method, population, or theory rather than paper-by-paper summaries.
- Compare and contrast studies.
- Make the gap explicit.
- Do not over-review literature that does not support the research problem.
- End by showing what your paper adds.
- For clinical journals, keep related work concise and clinically motivated.
- For CS journals, explicitly compare architectures, priors, losses, representations, and evaluation settings.
- For MRI technology journals, compare acquisition, reconstruction, preprocessing, biomarker quantification, and reproducibility.
- Always state whether prior work used internal testing, external testing, multi-center data, or reader/clinical evaluation.

### Useful Signals

- Previous studies have focused on ...
- In contrast, ...
- However, ...
- Whereas ...
- Little is known about ...
- This leaves open the question of ...
- The present study extends this work by ...

### Checklist

- [ ] Major research streams are identified.
- [ ] Existing contributions are fairly represented.
- [ ] Limitations or gaps are explicit.
- [ ] The present study is positioned clearly.
- [ ] The section supports the research question.
- [ ] Prior methods are compared on task, data, validation, and clinical relevance.
