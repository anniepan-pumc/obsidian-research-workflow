<%*
const sectionName = "03 Background";
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
title: "<% paperTitle %> - 03 Background"
type: writing-section
paper_title: "[[<% safePaperTitle %>|<% paperTitle %>]]"
paper_title_text: "<% paperTitle %>"
section: "03 Background"
created: <% tp.date.now("YYYY-MM-DD") %>
---
Parent paper: [[<% safePaperTitle %>|<% paperTitle %>]]

## Background

### Purpose

The Background section gives readers the concepts, context, and field-level problem needed to understand why the study matters.

### Content

- Disease significance:
- Clinical workflow or diagnostic challenge:
- Neurobiological or vascular mechanism:
- MRI modality and what it measures:
- Existing clinical or imaging biomarkers:
- Practical, theoretical, clinical, or methodological importance:
- Why this issue matters now:

### Domain-Specific Prompts

Neurodegenerative disease:

- What pathology, phenotype, subtype, or cognitive domain is central?
- Which biomarker is clinically meaningful: amyloid, tau, atrophy, iron, white matter integrity, vascular burden, perfusion, or connectivity?
- Is the paper about diagnosis, early detection, staging, subtype identification, prognosis, or treatment monitoring?

Cerebral vascular disease:

- Which vascular feature is central: vessels, EPVS, lacunes, microbleeds, white matter hyperintensities, stenosis, aneurysm, perfusion, or collateral circulation?
- Is the method intended to quantify burden, reconstruct anatomy, detect lesions, or explain clinical outcome?

MRI technology:

- Which sequence or derived map is used: T1, T2, FLAIR, DWI/DTI, SWI/QSM, MRA, ASL, fMRI, PET-MRI?
- What does the signal represent biologically or physically?
- What artifacts, resolution limits, scanner differences, or preprocessing steps matter?

### Draft Structure

Opening context:

> **Field/problem** is important because **reason**.

Known foundation:

> Existing research has established that **accepted knowledge**.

Specific context:

> In a specific population/system/setting, a specific issue is particularly relevant because of a **reason**.

Transition to gap:

> Despite this progress, the **unresolved issue** remains unclear.

MRI-specific bridge:

> Although **MRI modality/biomarker** can capture **tissue property or vascular feature**, its use for **tasks or clinical decisions is limited by technical or clinical limitations.

### Rules

- Include only background needed to understand the research problem.
- Define specialized terms early.
- Prefer synthesis over chronological summary.
- Use present tense for accepted knowledge.
- Use citations to establish credibility and scope.
- Explain MRI-derived metrics in clinically meaningful language.
- Avoid turning Background into Methods; keep acquisition details for Methods.
- For interdisciplinary readers, define both clinical abbreviations and algorithmic terms.

### Checklist

- [ ] Explains why the topic matters.
- [ ] Defines essential concepts.
- [ ] Provides enough context without becoming a full literature review.
- [ ] Leads naturally into the gap or related works section.
- [ ] Connects MRI signal or computational output to a disease-relevant interpretation.
