---
{"dg-publish":true,"permalink":"/developer/CSS/Hide Reveal Error Message/","dgPassFrontmatter":true}
---

clever way to animate an error message in and out. The error element is hidden when there is no inner text or html present, but once filled with content it will be revealed to the UI

```scss
.error{
    background-color: red;
    color: white;
    padding: .3em;
    margin-bottom: 1em;
    transition: .7s;
    
    &:empty{
      background-color: green;
      z-index: -500;
      height: 0;
      padding: 0;

    }
  }
```


---

## Backlinks
- [[developer/CSS/Cascading Style Sheet Tips & Tricks\|Cascading Style Sheet Tips & Tricks]]