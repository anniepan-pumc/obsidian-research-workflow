<%*
const title = await tp.system.prompt("Paper title");
const paperType = await tp.system.prompt("Paper type, e.g. research article, review, perspective", "research article");

if (!title) {
  new Notice("Paper title is required.");
  return;
}

const created = tp.date.now("YYYY-MM-DD");
const timestamp = tp.date.now("YYYYMMDD-HHmmss");

const rootFolder = "Writing";

// Sanitize file name
const safeTitle = title
  .replace(/[\\/:*?"<>|#^[\]]/g, "")
  .replace(/\s+/g, " ")
  .trim();

const fileName = safeTitle || timestamp;
const baseFolder = `${rootFolder}/${fileName}`;
const sectionTemplates = [
  "01 Writing Guidance",
  "02 Introduction",
  "03 Background",
  "04 Related Works",
  "05 Methods",
  "06 Results + Discussion",
  "07 Abstract + Title",
  "08 Final Check"
];

// Create folders if they do not exist
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

function renderSectionTemplate(raw, sectionName) {
  const scriptStart = "<" + "%*";
  const scriptEnd = "-" + "%" + ">";
  const tag = (expr) => "<" + "% " + expr + " %" + ">";
  let content = raw;

  if (content.startsWith(scriptStart)) {
    const scriptEndIndex = content.indexOf(scriptEnd);
    if (scriptEndIndex >= 0) {
      content = content.slice(scriptEndIndex + scriptEnd.length).trimStart();
    }
  }

  return content
    .replaceAll(tag("paperTitle"), title)
    .replaceAll(tag("safePaperTitle"), fileName)
    .replaceAll(tag('tp.date.now("YYYY-MM-DD")'), created)
    .replaceAll(tag("tp.date.now('YYYY-MM-DD')"), created)
    .replaceAll(tag("sectionName"), sectionName);
}

const createdSections = [];
const skippedSections = [];

for (const sectionName of sectionTemplates) {
  const templatePath = `Templates/Paper Helper/${sectionName}.md`;
  const targetFileName = `${fileName} - ${sectionName}`;
  const targetFilePath = `${baseFolder}/${targetFileName}.md`;

  if (await app.vault.adapter.exists(targetFilePath)) {
    skippedSections.push(targetFileName);
    continue;
  }

  const rawTemplate = await app.vault.adapter.read(templatePath);
  const sectionContent = renderSectionTemplate(rawTemplate, sectionName);
  await app.vault.create(targetFilePath, sectionContent);
  createdSections.push(targetFileName);
}

const createdSectionLinks = createdSections
  .map((section) => `- [[${section}]]`)
  .join("\n");

const skippedSectionLinks = skippedSections
  .map((section) => `- [[${section}]]`)
  .join("\n");

// Move the newly created note to Writing/<paper title>/<paper title>
let targetPath = `${baseFolder}/${fileName}`;
if (app.vault.getAbstractFileByPath(`${targetPath}.md`)) {
  targetPath = `${targetPath} ${tp.date.now("YYYYMMDD-HHmmss")}`;
}

await tp.file.move(targetPath);
-%>
---
title: "<% title %>"
type: writing
paper_type: "<% paperType %>"
status: false
created: <% created %>
target_journal:
word_limit:
keywords: []
---
# <% title %>

## 1. Drafting Dashboard

### Writing Order
1. Methods
2. Results + Discussion
3. Introduction
4. Abstract + Title
5. Final consistency check
6. Revision based on reviewer/editor comments

### Section Progress
| Section | Status | Last Edited | Last Action | Next Action | Linked Note |
|---|---|---:|---|---|---|
| Writing Guidance | not started |  |  |  | [[<% fileName %> - 01 Writing Guidance]] |
| Background | not started |  |  |  | [[<% fileName %> - 03 Background]] |
| Related Works | not started |  |  |  | [[<% fileName %> - 04 Related Works]] |
| Methods | not started |  |  |  | [[<% fileName %> - 05 Methods]] |
| Results + Discussion | not started |  |  |  | [[<% fileName %> - 06 Results + Discussion]] |
| Introduction | not started |  |  |  | [[<% fileName %> - 02 Introduction]] |
| Abstract + Title | not started |  |  |  | [[<% fileName %> - 07 Abstract + Title]] |
| Final Check | not started |  |  |  | [[<% fileName %> - 08 Final Check]] |
| Revision Based on Comments | not started |  |  |  | [[<% fileName %> - 09 Revision Based on Comments]] |

### Edit Movement Log
Use this log at the end of each writing session so the next session starts quickly.

| Date | Section | What I Changed | Why I Changed It | Evidence / File / Figure | Next Step |
|---|---|---|---|---|---|
| <% created %> | Dashboard | Created paper workspace and section notes | Started drafting structure | Auto-created subtemplates | Fill core message and choose first writing section |

### Auto-Created Section Notes

<% createdSectionLinks || "No new section notes were created." %>

---
## 2. Core Message
One-sentence summary:
> This paper argues that ...

### Research Problem
Background:
Gap in existing literature:
Problem statement:
> The key problem addressed in this paper is ...

### Project Type
- Primary audience: clinical neurology / neuroradiology / biomedical engineering / computer vision / MRI physics
- Paper style: clinical neuroimaging study / MRI biomarker study / deep learning method / computational image analysis / research letter / technical note
- Disease context: Alzheimer disease / neurodegenerative disease / cerebral small vessel disease / stroke / vascular disease / other
- Imaging modality: structural MRI / DWI / QSM / MRA / PET-MRI / multimodal MRI / other
- Method contribution: new model / new biomarker / validation study / external testing / clinical association / segmentation or reconstruction / synthesis or prediction
- Clinical contribution: diagnosis / prognosis / risk stratification / treatment monitoring / workflow triage / mechanistic insight

### Logical Flow
```text
Problem
→ Gap
→ Research question
→ Method/approach
→ Evidence
→ Findings
→ Interpretation
→ Contribution
```


## 3.  References
1. 
