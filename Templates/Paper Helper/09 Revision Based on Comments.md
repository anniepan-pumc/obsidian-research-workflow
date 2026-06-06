<%*
const sectionName = "09 Revision Based on Comments";
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
title: "<% paperTitle %> - 09 Revision Based on Comments"
type: writing-section
paper_title: "[[<% safePaperTitle %>|<% paperTitle %>]]"
paper_title_text: "<% paperTitle %>"
section: "09 Revision Based on Comments"
created: <% tp.date.now("YYYY-MM-DD") %>
revision_round:
journal:
decision:
deadline:
---
Parent paper: [[<% safePaperTitle %>|<% paperTitle %>]]

## Revision Based on Comments

### Revision Snapshot

- Journal:
- Manuscript ID:
- Decision: minor revision / major revision / revise and resubmit / rejected with invitation
- Revision round:
- Deadline:
- Main revision strategy:
- Biggest risk:
- New analyses required:
- New experiments required:
- Files to update: manuscript / response letter / cover letter / supplement / figures / tables / code / data availability

### Overall Response Strategy

Core response message:

> We thank the editor and reviewers for their constructive comments. We have revised the manuscript to clarify the main issue, strengthen the methods, results, and interpretation, and address specific concerns.

Revision priorities:

1. Address all requested changes explicitly.
2. Separate factual corrections, new analyses, textual clarification, and limitations.
3. Keep a respectful tone even when disagreeing.
4. Point reviewers to exact manuscript locations.
5. Avoid introducing unsupported claims during revision.

### Editor Comments

#### Editor Comment 1

Comment:

> **paste editor comment**

Response:

> Thank you for this comment. We have **revision action**. The revised text can be found in **section/page/line**.

Manuscript change:

> **new or revised sentence/paragraph**

Status:

- [ ] Addressed in response letter
- [ ] Addressed in manuscript
- [ ] Checked against other sections

---
### Reviewer 1

#### Comment 1.1

Comment type: conceptual / methodological / statistical / clinical interpretation / writing / figure-table / reference / limitation

Comment:

> **paste reviewer comment**

Action plan:

- What the reviewer is really asking:
- Required change:
- New analysis or experiment:
- Manuscript section to revise:
- Figure/table/supplement to revise:

Response draft:

> We thank the reviewer for raising this important point. **Direct answer to concern**. To address this, we **specific revision or analysis**. The results show **key result if applicable**. We have revised **section** to clarify **point**.

Manuscript change:

> **insert revised text**

Location:

- Section:
- Page:
- Line:
- Figure/table/supplement:

Status:

- [ ] Response drafted
- [ ] Manuscript revised
- [ ] New analysis checked
- [ ] Figure/table updated
- [ ] Supplement updated
- [ ] Cross-references updated

#### Comment 1.2

Comment:

> **paste reviewer comment**

Response:

> **response**

Manuscript change:

> **revised text**

Status:

- [ ] Done

---
### Reviewer 2

#### Comment 2.1

Comment:

> **paste reviewer comment**

Response:

> **response**

Manuscript change:

> **revised text**

Status:

- [ ] Done

---
### Common Response Patterns

Agreement and revision:

> We agree with the reviewer and have revised the manuscript accordingly.

Clarification without new analysis:

> We apologize for the lack of clarity. We have revised the text to clarify that **clarified point**.

New analysis:

> Following the reviewer’s suggestion, we performed an additional analysis of **analysis**. The new results show **finding**, which supports/qualifies **interpretation**.

Limitation acknowledged:

> We agree that **issue** is an important limitation. We have added this point to the Discussion and moderated the corresponding claim.

Respectful disagreement:

> We appreciate the reviewer’s perspective. However, **reasoned explanation based on data/literature/design**. To avoid overstatement, we have revised the manuscript to state **balanced wording**.

Unable to perform requested analysis:

> We agree that **requested analysis** would be valuable. However, this was not feasible because **specific reason**. We have now acknowledged this limitation and proposed it as an important direction for future work.

### MRI / AI / Neurology Revision Checklist

- [ ] Cohort description and inclusion/exclusion criteria are clearer.
- [ ] MRI acquisition and preprocessing details are sufficient.
- [ ] Train/validation/test split and data leakage prevention are explicit.
- [ ] Reference standard, label definition, or diagnostic criteria are clarified.
- [ ] External validation limitations are stated honestly.
- [ ] Statistical covariates and multiple comparison correction are justified.
- [ ] Baselines and ablations are adequate for technical claims.
- [ ] Clinical relevance is described without overclaiming deployment.
- [ ] Failure cases, uncertainty, or generalizability are discussed.
- [ ] Figures/tables match revised text and response letter.

### Manuscript Change Log

| Comment | Section | Change Made | New Analysis | Figure/Table | Status |
|---|---|---|---|---|---|
| Editor 1 | **section** | **change** | no | no | pending |
| Reviewer 1.1 | **section** | **change** | yes/no | **figure/table** | pending |

### Response Letter Skeleton

Dear Editor and Reviewers,

We thank you for the careful evaluation of our manuscript, **paper title**, and for the constructive comments. We have revised the manuscript in response to these comments. Major changes include:

1. **major revision 1**
2. **major revision 2**
3. **major revision 3**

Below, we provide a point-by-point response. Reviewer comments are reproduced in italic, followed by our responses.

Sincerely,

**corresponding author / authors**

### Final Revision Check

- [ ] Every comment has a direct response.
- [ ] Every promised change is present in the manuscript.
- [ ] Page/line numbers are updated after final formatting.
- [ ] Tone is professional and appreciative.
- [ ] Claims are moderated where reviewers questioned overstatement.
- [ ] Abstract, title, figures, tables, and supplement reflect revised results.
- [ ] Tracked changes or highlighted changes are prepared if required.
- [ ] Clean manuscript and marked manuscript are both ready if required.
