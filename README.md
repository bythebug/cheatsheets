# Cheatsheets

Personal notes I made for my own interview prep.

**Live site → [bythebug.github.io/cheatsheets](https://bythebug.github.io/cheatsheets/)**

## Sheets

| Sheet | File |
|-------|------|
| DSA Cheat Sheet | `sheets/dsa.md` |

## Adding a new sheet

1. Create `sheets/your-topic.md`
2. Add frontmatter at the top:

```yaml
---
layout: sheet
title: Your Topic
description: One-line description
---
```

3. Write content in Markdown — code blocks, tables, callouts all work
4. Add a link in `index.md`
5. `git add . && git commit -m "add X sheet" && git push`

GitHub Pages builds and deploys automatically.
