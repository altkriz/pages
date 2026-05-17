
# KrizPages

A curated collection of hand-crafted HTML, CSS, and JS templates. Each page is a standalone creation—no frameworks, no build steps, no dependencies. Just clean code that works.

---

## 🌐 Live Showcase

View the gallery and preview templates live:

**[https://altkriz.github.io/pages/](https://altkriz.github.io/pages/)**

---

## ✨ Features

- **Pure Code** — No frameworks or heavy dependencies. Just raw HTML, CSS, and JS.
- **Live Previews** — Click any template to see it live in an instant iframe overlay.
- **Filter & Browse** — Easily filter templates by category (Landing, Dashboard, etc.).
- **Always Growing** — New pages and designs added regularly.

---

## 📂 Templates

| Page | Link | Description | Tags |
| :--- | :--- | :--- | :--- |
| Page 1 | [View Live](https://altkriz.github.io/pages/page1.html) | A sleek, modern landing page with smooth animations and bold typography. | `Landing`, `Dark` |
| Page 2 | [View Live](https://altkriz.github.io/pages/page2.html) | An interactive dashboard-style layout with data-driven components. | `Dashboard`, `Interactive` |
| Page 3 | [View Live](https://altkriz.github.io/pages/page3.html) | A creative portfolio layout with experimental CSS techniques. | `Portfolio`, `Creative` |

---

## 🛠️ Adding a New Template

The gallery is designed to be easily expandable. To add a new page:

1. Create your new `page4.html` file and add it to the root directory.
2. Open `index.html` and find the `pages` array inside the `<script>` section at the bottom.
3. Add a new object for your page following this format:

```javascript
{
  id: 4,
  title: "Page Four",
  description: "Your description here.",
  tags: ["Landing", "Minimal"],
  thumbnail: "https://picsum.photos/seed/kriz-p4/800/500.jpg"
}
```

4. Commit and push! The showcase grid, filter buttons, and stats will update automatically.

---

## 💻 Tech Stack

- **HTML5**
- **CSS3** (via Tailwind CSS CDN)
- **Vanilla JavaScript**
- **Iconify** (for icons)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
