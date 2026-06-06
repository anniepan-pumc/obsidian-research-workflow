<%*
const sectionName = "08 Final Check";
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
title: "<% paperTitle %> - 08 Final Check"
type: writing-section
paper_title: "[[<% safePaperTitle %>|<% paperTitle %>]]"
paper_title_text: "<% paperTitle %>"
section: "08 Final Check"
created: <% tp.date.now("YYYY-MM-DD") %>
---
Parent paper: [[<% safePaperTitle %>|<% paperTitle %>]]

## Final Consistency Check

### Cross-Section Alignment

- [ ] Title reflects the main topic, method/population, and contribution.
- [ ] Abstract matches the title and manuscript.
- [ ] Introduction gap is answered in Results and Discussion.
- [ ] Methods provide enough detail to evaluate and repeat the work.
- [ ] Results answer the stated research question.
- [ ] Discussion connects findings back to the Introduction.
- [ ] Conclusion gives the final answer and "so what".
- [ ] Key terms are consistent across title, abstract, introduction, results, and discussion.
- [ ] Disease context, MRI modality, method, and clinical task are named consistently.

### Claims and Evidence

- [ ] Every major claim is supported by data or references.
- [ ] Speculation is clearly marked.
- [ ] Limitations are acknowledged without undermining the study.
- [ ] No new results appear after the Results section.
- [ ] Tables and figures are cited in order.
- [ ] Abstract contains no unsupported information.
- [ ] Clinical utility claims are supported by the study design.
- [ ] Technical superiority claims are supported by appropriate baselines and metrics.

### Language

- [ ] Tense choices match sentence function.
- [ ] Voice matches target journal expectations.
- [ ] Jargon and abbreviations are controlled.
- [ ] Sentences are concise.
- [ ] Journal style for units, symbols, headings, and references is followed.

### MRI / AI / Neurology Reporting Check

- [ ] Cohort, inclusion/exclusion criteria, and demographics are clear.
- [ ] MRI acquisition and preprocessing are reproducible.
- [ ] Reference standard, labels, or clinical diagnosis criteria are defined.
- [ ] Train/validation/test splits avoid patient-level leakage.
- [ ] External validation or its absence is clearly stated.
- [ ] Metrics match the task and include uncertainty where appropriate.
- [ ] Statistical models include relevant covariates.
- [ ] Scanner/site/protocol variability is considered.
- [ ] Failure cases, limitations, and generalizability are discussed.
- [ ] Data/code availability, ethics, funding, and conflicts are ready for journal submission.

### Final Pass

- [ ] The paper has one clear central message.
- [ ] Each section contributes to that central message.
- [ ] The contribution is visible in the Introduction, Discussion, Abstract, and Title.
- [ ] The manuscript is ready for journal-specific formatting.
