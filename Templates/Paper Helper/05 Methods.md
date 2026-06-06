<%*
const sectionName = "05 Methods";
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
title: "<% paperTitle %> - 05 Methods"
type: writing-section
paper_title: "[[<% safePaperTitle %>|<% paperTitle %>]]"
paper_title_text: "<% paperTitle %>"
section: "05 Methods"
created: <% tp.date.now("YYYY-MM-DD") %>
---
Parent paper: [[<% safePaperTitle %>|<% paperTitle %>]]

## Methods

### Purpose

The Methods section should allow a trained reader to evaluate, trust, and repeat the work.

### Content

- Materials/ data / subjects/ samples:
- Selection criteria:
- Study design:
- Procedure:
- Measures/ instruments:
- Reliability and validity:
- Statistical or analytical approach:
- Ethics / consent / approvals:
- Software and versions:

### Recommended Structure for MRI + Computational Papers

#### Study Design and Cohort / Dataset

- Study design: retrospective / prospective / cross-sectional / longitudinal / technical validation / external validation
- Data source: hospital cohort / public dataset / multi-center dataset / synthetic data / phantom / animal model
- Inclusion criteria:
- Exclusion criteria:
- Demographics and clinical variables:
- Disease labels, diagnosis criteria, rating scales, cognitive tests, or outcome definitions:
- Train/validation/test split:
- External test set:
- Data leakage prevention:

#### MRI Acquisition and Image Preprocessing

- Scanner vendor, field strength, coil, and site:
	- Manufacturer
	- Scanner
	- field strength
	- coil name
- MRI protocols and key parameters: 
	- Resolution = ? mm^3
	- Voxel size = ? mm^3
	- Repetition time (TR) = ? ms
	- Echo time (TE) = ? ms
	- Inversion time (TI) = ? ms
	- flip angle = ? degree
	- Field of view (FOV) = ? mm^2
	- Slice spacing = ？
	- dMRI parameters: number of directions = ?; b value = ? s/mm^2; NEX =  ?
- Preprocessing: registration, skull stripping, bias correction, normalization, resampling, denoising, intensity normalization
- Region of interest or atlas:
- Quality control:
- Handling of artifacts, missing data, motion, and scanner differences:

#### Reference Standard / Labels

- Manual annotation, clinical diagnosis, radiology rating, pathology, PET status, cognitive score, or biomarker threshold:
- Rater training and inter-rater reliability:
- Label uncertainty or adjudication:

#### Computational Method / Model

- Task: segmentation / reconstruction / synthesis / classification / regression / association / biomarker extraction
- Input and output:
- Model architecture or mathematical model:
- Prior, regularization, loss function, or physical constraint:
- Training strategy:
- Hyperparameters:
- Implementation framework and hardware:
- Baseline methods:
- Ablation experiments:

#### Evaluation and Statistical Analysis

- Technical metrics: Dice, Hausdorff distance, sensitivity, specificity, AUC, MAE, RMSE, PSNR, SSIM, correlation, Bland-Altman, calibration
- Clinical metrics: diagnostic accuracy, subtype discrimination, association with cognition, odds ratio, hazard ratio, effect size
- Statistical tests and covariates:
- Multiple comparison correction:
- Confidence intervals:
- Subgroup analysis:
- Robustness and external validation:

### Paragraph Template

Purpose:

> The purpose of the procedure**, **sample/material** was/were **prepared/collected/analyzed**.

Procedure:

> First, **step**. Next, **step**. After **time/condition**, **step**. Finally, **measurement/analysis**.

Reliability/validity:

> Each experiment was repeated a number of times with **replicates/controls**. **Validation/calibration/statistical method** was used to **purpose**.

Statistical analysis:

> Data were analyzed using **test/model** in **software/version**. A **two-sided/one-sided** P value of **threshold** was considered statistically significant.

Ethics:

> The study was approved by **committee** and conducted according to **guideline**. Written informed consent was obtained from **participants**, where applicable.

Deep learning method:

> We trained **model architecture** to map **input MRI/modality** to **target output**. The model was optimized using **loss function** with **training strategy**, and performance was compared with **baseline methods** on **internal and external test sets**.

Clinical MRI analysis:

> MRI-derived **biomarker/measure** was quantified in **regions/cohort** and compared across **groups/subtypes**. Associations with **clinical variable/outcome** were assessed using **statistical model** adjusted for **covariates**.

### Rules

- Be specific and precise.
- Organize by chronology, procedure type, experiment, or study aim.
- Cite existing methods and state modifications.
- Explain methodological problems professionally.
- Use journal-preferred units, abbreviations, and symbols.
- State enough acquisition and preprocessing detail for reproducibility.
- Separate model development from model evaluation.
- Report how subjects, scans, slices, or patches were split to avoid leakage.
- For multi-site MRI, state how scanner/site effects were handled.
- For clinical readers, explain what each computational metric means clinically.
- For CS readers, include baselines, ablations, implementation details, and complexity when relevant.

### Tense and Voice

- Mostly past tense for completed procedures.
- Present tense for general validity and references to tables, figures, or appendices.
- Passive voice is common when procedures matter more than researchers.
- Active voice and first person are acceptable if the journal permits it.

### Checklist

- [ ] Reader can evaluate reliability and validity.
- [ ] Reader can repeat the work.
- [ ] Design, procedures, materials/data, and analysis are clear.
- [ ] Ethics and approvals are stated where relevant.
- [ ] Existing methods and modifications are cited.
- [ ] Units and terminology match journal style.
- [ ] MRI acquisition and preprocessing are reproducible.
- [ ] Dataset split and external validation are clearly described.
- [ ] Reference standard and label reliability are reported.
- [ ] Baselines, ablations, and metrics match the claimed contribution.
