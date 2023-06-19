
```dataviewjs
// Get all pages from the root (the vault)
const pages = dv.pages('"Mathematics"')

// This regex will find the contents of a specifically formatted callout
const regex = /\n(.*)\[\!Question\](.*)\n/

const rows = []
for (const page of pages) {
    const file = app.vault.getAbstractFileByPath(page.file.path)
    // Read the file contents
    const contents = await app.vault.read(file)
    // Extract the summary via regex
    for (const callout of contents.match(new RegExp(regex, 'sg')) || []) {
        const match = callout.match(new RegExp(regex, 's')) 
        rows.push([page.file.link])
    }
}

dv.table(['Link'], rows)
```
