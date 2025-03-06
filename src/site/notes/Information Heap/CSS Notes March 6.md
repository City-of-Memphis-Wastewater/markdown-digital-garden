---
{"dg-publish":true,"permalink":"/information-heap/css-notes-march-6/","noteIcon":"","created":"2025-03-06T15:52:40.486-06:00"}
---

Date: [[-Daily Activity Log-/2025 03-March 06\|2025 03-March 06]]

# Query:
generate a styles.css files, which includes a dark mode and a light mode. Use Shades of green and burnt orange for accents.

# Copilot:
Absolutely, Clayton! I'll create a `styles.css` file for you that includes both dark and light modes, with shades of green and burnt orange for accents. 

Here you go:

```css
/* Define colors for light mode */
:root {
    --background-color-light: #FFFFFF;
    --text-color-light: #000000;
    --accent-color-light: #A5D6A7; /* light green */
    --accent-color-alt-light: #FFB74D; /* burnt orange */
}

/* Define colors for dark mode */
:root.dark-mode {
    --background-color-dark: #121212;
    --text-color-dark: #FFFFFF;
    --accent-color-dark: #66BB6A; /* dark green */
    --accent-color-alt-dark: #FF7043; /* burnt orange */
}

/* Light mode styles */
body {
    background-color: var(--background-color-light);
    color: var(--text-color-light);
}

a {
    color: var(--accent-color-light);
}

a:hover {
    color: var(--accent-color-alt-light);
}

/* Dark mode styles */
body.dark-mode {
    background-color: var(--background-color-dark);
    color: var(--text-color-dark);
}

a.dark-mode {
    color: var(--accent-color-dark);
}

a.dark-mode:hover {
    color: var(--accent-color-alt-dark);
}

/* Toggle dark mode */
.dark-mode-toggle {
    cursor: pointer;
    padding: 10px;
    background-color: var(--accent-color-light);
    color: var(--text-color-light);
    border: none;
}

.dark-mode-toggle.dark-mode {
    background-color: var(--accent-color-dark);
    color: var(--text-color-dark);
}
```

To toggle between dark mode and light mode, you can use JavaScript to add or remove the `dark-mode` class from the `<body>` element. Here's an example:

```html
<button class="dark-mode-toggle" onclick="toggleDarkMode()">Toggle Dark Mode</button>

<script>
function toggleDarkMode() {
    document.body.classList.toggle('dark-mode');
    let links = document.querySelectorAll('a');
    links.forEach(link => {
        link.classList.toggle('dark-mode');
    });
    let toggleButton = document.querySelector('.dark-mode-toggle');
    toggleButton.classList.toggle('dark-mode');
}
</script>
```

This setup provides a simple way to switch between dark and light modes with shades of green and burnt orange accents. Let me know if you need any further adjustments!