---
{"dg-publish":true,"permalink":"/developer/NodeJS/CSS Modules vs Styled Components/","dgPassFrontmatter":true}
---

At first I was 100% all in on [[developer/NodeJS/styled-components\|styled-components]]. I thought I wouldn't need anything else when it came to styling my markdown. As my projects grew in complexity and asked for more modularity I began to rethink the way I structure my code base. 

## Why Styled Components
The big pro for styled components is pragmatically changing the styles by passing props to the component. This does open up some great flexibility,  but at a cost of more lines of code. If I needed to programmatically change the styles I'd just reach for [[developer/ReactJS/Mixing Style Objects Inline\|Inline styles]] with #JSX. 

## CSS Modules
In this case I'm using `styles.module.scss` for reasons below

1. my style files are always separated (for modularity)
2. less dependent on dependancies
3. production builder is better optimized
4. native intellisense with code editors like [[developer/VS Code\|VS Code]]. (Yes, I know there are plugins for styled components)
	1. Better auto complete / commenting out in regular `css` files
5. less plugins - the browser dev tools shows you the class names natively

the caveat is targeting custom named css selector classes that are nested in the parent. That's where `:global` comes in as a work around

```scss
article.thumbnail :global {
  display: flex;
  flex-direction: column;
  background-color: var(--c-txt-rev);
  height: 100%;
  box-shadow: var(--boxs-1);
  // max-width: 50em;

  .featured_image{
    transition: transform, contrast, .3s;
    
    &:hover, &:focus{
      transform: scale(1.01);
      filter: contrast(1.2);
    }
  }
}
```

## Conclusion
I'm still using a combination of the two, but **CSS Modules** will be the first tech I reach for. I'll definitely miss the auto imports when typing out something like `<StyledCard />` into my JSX, but the convenience is negligible to the benefits from Modules 

---
## Backlinks
- [[developer/NodeJS/NodeJS\|NodeJS]]
- [[developer/ReactJS/ReactJS\|ReactJS]]
- [[developer/NextJS/NextJS\|NextJS]]