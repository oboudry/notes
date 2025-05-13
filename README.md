# Notes

A simple, fast, versioned note-taking system built with [Hugo](https://gohugo.io/), [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme, and hosted on [GitHub Pages](https://pages.github.com/).

> ✍️ Write your notes in Markdown, version them with Git, and instantly publish as a beautiful static website.

---

## 🚀 Quick Start

### 1. Clone this repository

```bash
git clone https://github.com/YOURUSERNAME/notes.git
cd notes
```

### 2. Install Hugo

[Install Hugo](https://gohugo.io/getting-started/installing/) if you don't have it yet.

### 3. Write a Note

Create a new note:

```bash
hugo new note/my-first-note.md
```

Edit the note (see `content/note/`), set `draft: false` when ready to publish.

### 4. Run and Preview Locally

```bash
hugo server
```
Browse to [http://localhost:1313](http://localhost:1313) to preview.

### 5. Build and Publish

```bash
hugo            # Builds the static site into /docs
git add .
git commit -m "Add new note"
git push
```
Your site will be updated at:  
`https://YOURUSERNAME.github.io/notes/`

---

## ✨ Features

- Write in Markdown, publish instantly
- Fast, beautiful site with search & theme (PaperMod)
- Served for free via GitHub Pages
- Navigation bar to quickly access home, notes, and about pages
- All content is versioned and backed up in Git

---

## 🔧 Configuration

- **Site menus**: See [`config.toml`](config.toml), edit the `[menu]` section to customize navigation.
- **About page**: Edit `content/about.md`
- **Notes**: Add Markdown files to `content/note/`. Remove or set `draft: false` to publish.

---

## 💡 Tips

- Use `hugo server -D` to preview draft notes locally.
- Organize notes in subfolders for topics or tags.
- For more theme options and configuration, see [PaperMod documentation](https://adityatelange.github.io/hugo-PaperMod/).

---

## 📦 Advanced: GitHub Actions (Optional)

If you use GitHub Actions for auto-deployment, see `.github/workflows/` for configuration.

---

## License

This project is licensed under the [MIT License](LICENSE).
