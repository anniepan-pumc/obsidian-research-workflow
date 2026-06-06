<%*
const sectionName = "06 Results + Discussion";
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
title: "<% paperTitle %> - 06 Results + Discussion"
type: writing-section
paper_title: "[[<% safePaperTitle %>|<% paperTitle %>]]"
paper_title_text: "<% paperTitle %>"
section: "06 Results + Discussion"
created: <% tp.date.now("YYYY-MM-DD") %>
---
Parent paper: [[<% safePaperTitle %>|<% paperTitle %>]]

## Results + Discussion

### Results Purpose

The Results section reports what was found and gives meaning to the data. Do not present raw data alone.

### Results Organization

Choose one sequence:

- Most important to least important.
- Order of research questions or hypotheses.
- Chronological order.
- Order of experiments or analyses.

Recommended order for MRI + computational papers:

1. Cohort/dataset characteristics and image quality.
2. Primary technical performance or primary clinical association.
3. External validation or subgroup analysis.
4. Ablation, baseline comparison, or sensitivity analysis.
5. Qualitative examples, visualization, or error analysis.
6. Clinical interpretation.

### Results Segment Template

Purpose/background if needed:

> To determine whether **question**, we **experiment/analysis**.

Approach if needed:

> **Sample/system** was **tested/measured/compared** under **conditions**.

Key result:

> **Outcome** increased/decreased/differed/remained unchanged from **value** to **value** under **condition** (Figure/Table **number**).

Brief interpretation:

> These results indicate/suggest/imply that **direct meaning of result**.

Hedge if needed:

> This pattern may suggest **interpretation**, although **constraint**.

Deep learning performance:

> The proposed model achieved **metric values** on **test set**, outperforming **baseline** by **difference**. Performance was consistent/inconsistent across **external site, scanner, disease group, or subgroup**.

MRI biomarker association:

> Higher/lower **MRI biomarker** was associated with **clinical outcome or disease subtype** after adjustment for **covariates**.

Qualitative or visual assessment:

> Representative cases showed that **method/model** preserved/captured **anatomical or pathological feature**, whereas failures occurred mainly in **failure condition**.

### Tables and Figures

- Use tables for exact values and multi-variable comparisons.
- Use figures for patterns, trends, relationships, images, maps, or diagrams.
- Number tables and figures in order of first mention.
- Table titles go above tables; figure captions go below figures.
- Captions and titles should stand alone.
- Do not duplicate the same information in both a table and a figure.

Common figures:

- Study flowchart or cohort diagram.
- Model architecture or processing pipeline.
- MRI examples with overlays, heatmaps, synthetic images, vessel reconstructions, or biomarker maps.
- Quantitative performance table across internal and external sets.
- Bland-Altman, correlation, ROC, calibration, or subgroup plots.
- Error/failure examples.

### Discussion Purpose

The Discussion interprets the significance of findings, connects back to the Introduction, and moves the reader's understanding forward. It is not a second Results section.

### Discussion Structure

Main answer:

> This study shows/demonstrates/suggests that **answer to research question**.

Technical and clinical answer:

> Technically, **method/model/biomarker** achieved **primary technical result**. Clinically, this suggests that **clinical interpretation or potential use**, although **validation requirement** remains necessary.

Key findings:

> The main findings were **key finding 1**, **key finding 2**, and **key finding 3**.

Context:

> Our results are consistent with **previous studies** in showing that **shared finding**.
> In contrast to **previous studies**, we found that **difference**.

Explanation:

> One possible explanation is **mechanism/methodological reason/context**.

Unexpected results:

> Unexpectedly, **finding**. This may be explained by **reason**, although **alternative/limitation** should also be considered.

Limitations:

> This study has several limitations. First, **limitation**, which may **impact**. Nevertheless, **strength/mitigation**.

Implications and future work:

> These findings may inform **practice/policy/model/theory** by **specific implication**.
> Future studies should **next step** to determine whether **unresolved question**.

MRI/AI-specific limitations:

> Limitations include **sample size, single-center data, scanner heterogeneity, retrospective design, label uncertainty, class imbalance, limited ethnic or disease diversity, absence of pathology confirmation, or lack of prospective testing**.
> Future work should evaluate **method/model/biomarker** in **multi-center prospective cohorts** and assess **clinical workflow impact or decision utility**.

### Rules

- Results: report and lightly interpret what the data show.
- Discussion: explain why findings matter and place them in context.
- Provide the answer to the research question early in the Discussion.
- Compare findings with previous studies.
- Discuss limitations without undermining the study.
- Do not introduce new results in the Discussion.
- Do not overstate claims beyond the data.
- Distinguish technical accuracy from clinical utility.
- Include error analysis for computational methods.
- Discuss generalizability across scanner, site, protocol, population, and disease subtype.
- For synthesis tasks, avoid implying that synthetic images replace real clinical scans unless proven.
- For biomarker studies, explain biological plausibility and clinical relevance.

### Tense

- Results from this study: past tense.
- What tables/figures show: present tense is often acceptable.
- Discussion answer, interpretation, and significance: present tense.
- Completed procedures and specific findings: past tense.

### Checklist

- [ ] Results answer the research question.
- [ ] Main findings appear early.
- [ ] Raw data are converted into meaningful result statements.
- [ ] Tables and figures are necessary and cited in order.
- [ ] Discussion starts with the main answer.
- [ ] Findings are compared with previous studies.
- [ ] Limitations and implications are realistic.
- [ ] No new results are introduced in Discussion.
- [ ] External validation, subgroup behavior, or robustness is reported if claimed.
- [ ] Failure cases and sources of error are described.
- [ ] Clinical translation claims are supported by the study design.
