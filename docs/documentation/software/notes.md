---
tags:
    - software
    - documentation
    - geany
    - text editor
    - markdown
    - vs code
---
# Notes

The objection about syncing can be eliminated if you are using Nextcloud Desktop OR 
A Git repo for notes that you manually sync.

---

### **1. Geany with Markdown Plugin (8.5)**  
- Geany is known for being extremely fast and responsive. If you're looking for a minimal tool that doesn't consume a lot of system resources, especially on older or low-powered machines, Geany is a great option.  It starts up quickly, and you can edit large markdown files without experiencing lag or slowdown.

- Geany is highly customizable. You can add various plugins to extend its functionality, including the Markdown plugin, which adds syntax highlighting and live previews. You can also customize the interface, key bindings, and workflows to match your note-taking style. Since Geany is an IDE, it offers more flexibility than traditional note-taking apps, and you can configure it to suit your workflow.

- With the markdown plugin, you get syntax highlighting, making it easier to write and edit markdown files. While it's not as advanced as dedicated markdown editors (like Obsidian or Typora), it's sufficient for most markdown editing tasks.

- For those who like a clean, distraction-free writing environment, Geany’s minimal interface can be a positive. It doesn’t come with too many bells and whistles, so you can focus on the content itself.

- Geany supports multiple open files in tabs, which can be helpful for keeping several markdown notes open at once. It also has features like file tree navigation, which makes it easy to organize and find your notes within a project structure.

---

### **2. Neovim with `iamcco/markdown-preview.nvim` (8.4)**  
Neovim, paired with the **markdown-preview.nvim** plugin, offers a highly customizable, efficient markdown editing experience. Ideal for users who prefer command-line tools and have a comfort level with Vim-style editing. The live preview functionality in the browser, along with the power of plugins like **Telescope** and **Treesitter**, makes it a powerful choice for advanced users. However, it requires setup and a steeper learning curve compared to more GUI-based editors.

!!! tip "Simple Neovim config"

	[My Neovim config](https://github.com/drewgrif/nvim)
---

### **3. VSCode (vscodium) with Plugins (8.1)**  
VSCode is a feature-rich, highly customizable editor with a wide range of plugins, making it ideal for users who want an all-in-one solution. While it offers robust markdown editing features, its performance can be a bit heavy for users who are simply looking for a lightweight markdown editor. The initial setup and complexity of managing plugins may deter casual users.

---

### **4. Iotas (7.9)**  
Iotas offers a modern, simple interface with markdown support and focuses on lightweight note organization. It's great for basic markdown writing, but lacks advanced features like backlinks or graph views that some other note-taking apps offer.

---

### **5. Obsidian (7.8)**  
Obsidian is perfect for users who want to create a network of interconnected notes with advanced features like backlinks and graph view. However, it can become resource-heavy with large vaults and has a learning curve due to its complexity and reliance on plugins.

---

### **6. Apostrophe (7.5)**  
Apostrophe provides a clean, distraction-free writing experience, with full markdown support and a live preview. It's great for writers but lacks more advanced features like task management and note linking that would make it more powerful for complex note-taking.

---

### **7. Zettlr (7.4)**  
Zettlr is well-suited for academic writing, as it offers built-in citation management along with markdown support. While the feature set is great for researchers, its complex UI and lack of built-in syncing make it less appealing for casual note-takers.

---

### **8. MarkText (7.2)**  
MarkText has a clean and user-friendly interface with live preview, making it easy for users to edit markdown documents. However, it lacks advanced features like backlinks, and it's no longer actively maintained, which could lead to stability issues over time.

---

### **9. Joplin (6.5)**  
Joplin is an open-source note-taking tool with end-to-end encryption and cross-device syncing. Despite its strong security features, its outdated interface, performance issues with large databases, and lack of advanced note-linking features make it less competitive for users looking for modern, feature-rich note-taking apps.

---

### **10. QOwnNotes (5.8)**  
QOwnNotes supports markdown and integrates with Nextcloud for syncing, but its outdated UI and limited features make it less attractive compared to other tools. Its lack of mobile support and occasional syncing issues with Nextcloud are notable drawbacks.

---

### **11. Logseq (5.0)**  
Logseq is an outliner-based tool that focuses on hierarchical note-taking and knowledge management. It’s a unique approach, but its steep learning curve, clunky UI, and performance issues with larger notes make it a less practical option for many users.

---

### **Conclusion:**
Now with **Neovim (8.4)** and **VSCode (8.1)** swapped in the rankings, **Geany** remains the top contender, thanks to its speed and lightweight design. **Neovim** rises to second place due to its customization, speed, and power for users comfortable with command-line environments, while **VSCode** drops to third due to its complexity and resource usage. Each tool offers distinct advantages, so the best choice depends largely on your preferences and technical comfort level.

