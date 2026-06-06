<%*
const term = await tp.system.prompt("Term or phrase");
const field = await tp.system.prompt("Field, e.g. Machine Learning, Neuroscience, Statistics");

if (!term || !field) {
  new Notice("Term and field are required.");
  return;
}

const baseFolder = "Knowledge Summary/Words and Phrases";
const targetFolder = `${baseFolder}/${field}`;

// Sanitize file name
const safeTerm = term
  .replace(/[\\/:*?"<>|#^[\]]/g, "")
  .replace(/\s+/g, " ")
  .trim();

// Create folders recursively if they do not exist
async function ensureFolder(path) {
  const parts = path.split("/");
  let currentPath = "";

  for (const part of parts) {
    currentPath = currentPath ? `${currentPath}/${part}` : part;

    if (!app.vault.getAbstractFileByPath(currentPath)) {
      await app.vault.createFolder(currentPath);
    }
  }
}

await ensureFolder(targetFolder);

// Move the newly created note to the target field folder
await tp.file.move(`${targetFolder}/${safeTerm}`);
-%>
---
title: "<% term %>"
field: "<% field %>"
type: term-explanation
status: false
created: <% tp.date.now("YYYY-MM-DD") %>
---

# <% term %>

## Definition

## Key Ideas

## Explanation
Provide a clear explanation of the concept in plain language.
## Applications

## Notes

## References
- 