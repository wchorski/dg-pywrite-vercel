---
{"dg-publish":true,"tags":["css","style","theme","darkmode"],"permalink":"/developer/CSS/Handle Dark Light Themes The Right Way/","dgPassFrontmatter":true}
---

Respect a user's system settings by setting up your [[developer/CSS/CSS\|CSS]] in a dynamic way. 

## Conflicts
Sometimes a website **owner** wants their site to be defaulted to #darkmode  (even if the the **user** prefers a #lighttheme). With this logic, you can still have the site default to the **owner's** preferred theme, but give the **user** an option to flip back to a light theme with those trendy <button>☀️</button> <button>🌙</button> theme switcher buttons

```css
:root {
  --base: #cfcfcf;
  --text: #212121;
}

@media (prefers-color-scheme: dark) {
  :root {
    --base: #212121;
    --text: #cfcfcf;
  }
}

html {
  color-scheme: dark light;
}
/* CSS-only solution using :has()
   the JS version is commented out lower down */
:root:has(#theme [value="☀️"]:checked) {
  color-scheme: light;
  --base: #cfcfcf;
  --text: #212121;
}

:root:has(#theme [value="🌑"]:checked) {
  color-scheme: dark;
  --base: #212121;
  --text: #cfcfcf;
}

body {
  color: var(--text);
  background-color: var(--base);
}

body {
  font-family: system-ui;
  font-size: 1.5rem;
}

input,
select {
  font: inherit;
}

.with-scrollbar {
  height: 300px;
  border: 5px solid hsl(0 0 50% / 0.5);
  overflow-y: scroll;
}

/* Use the two selectors below if you want the JS version
[data-theme="☀️"] {
  color-scheme: light;
  --base: #cfcfcf;
  --text: #212121;
}

[data-theme="🌑"] {
  color-scheme: dark;
  --base: #212121;
  --text: #cfcfcf;
} */

```

## Credits
- [simple light/dark example (codepen.io)](https://codepen.io/kevinpowell/pen/KKEevOp
- [A simple mistake that can screw up your light/dark theme (and how to fix it) (youtube.com)](https://www.youtube.com/watch?v=zFFuV_vXNhY)