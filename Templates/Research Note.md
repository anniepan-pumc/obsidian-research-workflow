<%*
const titleInput = await tp.system.prompt("Research title");
const keywordsInput = await tp.system.prompt(
  "Keywords, separated by comma or semicolon, e.g. White Matter Hyperintensity; Clinical Statistics; Disease Progression Modeling",
  ""
);

if (!titleInput) {
  new Notice("已取消创建：title 为空。");
  return;
}

const title = titleInput.trim();
const created = tp.date.now("YYYY-MM-DD");
const filename = title;
const targetFolder = "Knowledge Summary/Research Records";

// 同时兼容英文逗号、中文逗号、英文分号、中文分号
function splitKeywords(input) {
  if (!input) return [];

  return input
    .split(/[,，;；]+/)
    .map(k => k.trim())
    .filter(k => k.length > 0);
}

const keywords = splitKeywords(keywordsInput);

function yamlQuote(str) {
  return `"${String(str).replace(/\\/g, "\\\\").replace(/"/g, '\\"')}"`;
}

// 将 keyword 转换为 Obsidian tag
// White Matter Hyperintensity -> #white-matter-hyperintensity
// Clinical Statistics -> #clinical-statistics
// Disease Progression Modeling -> #disease-progression-modeling
function keywordToTag(keyword) {
  return "#" + keyword
    .trim()
    .replace(/^#+/, "")
    .toLowerCase()
    .replace(/&/g, "and")
    .replace(/\s+/g, "-")
    .replace(/[^\p{L}\p{N}_/-]/gu, "")
    .replace(/-+/g, "-")
    .replace(/^-|-$/g, "");
}

const keywordTags = keywords
  .map(keywordToTag)
  .filter(tag => tag.length > 1);

// 去重，避免重复 tag
const uniqueKeywordTags = [...new Set(keywordTags)];

async function ensureFolder(path) {
  if (!app.vault.getAbstractFileByPath(path)) {
    await app.vault.createFolder(path);
  }
}

await ensureFolder(targetFolder);

// 自动移动并重命名当前新笔记到 Research Records 目录
await tp.file.move(`/${targetFolder}/${filename}`);

tR += `---
title: ${yamlQuote(title)}
`;

if (keywords.length > 0) {
  tR += `keywords:\n`;
  for (const k of keywords) {
    tR += `  - ${yamlQuote(k)}\n`;
  }
} else {
  tR += `keywords: []\n`;
}

tR += `status: false
creation date: ${created}
---

# ${title}

#### Aims

Keywords:

`;

if (uniqueKeywordTags.length > 0) {
  for (const tag of uniqueKeywordTags) {
    tR += `- ${tag}\n`;
  }
} else {
  tR += `- \n`;
}

tR += `

#### Research Design


#### Runtime configuration

- Programming languages and packages
- Computational resources
- Operating systems

#### Data


#### Methods


#### Results


#### Discussion
`;
-%>
