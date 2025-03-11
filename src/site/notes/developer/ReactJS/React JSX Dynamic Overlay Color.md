---
{"dg-publish":true,"tags":["reactjs","css","html"],"permalink":"/developer/ReactJS/React JSX Dynamic Overlay Color/","dgPassFrontmatter":true}
---

I wanted a dynamic way to add a transparent color that would tint the background image of a `<section>`. I went the the pseudo `::before` route as I thought it would keep my #html less cluttered.

Problem is, I don't like using CSS in JS libraries anymore as per my [[developer/NodeJS/CSS Modules vs Styled Components\|CSS Modules vs Styled Components]] thoughts.

## My React Component

in the component I add in a locally scoped css variable that I can call later

```jsx
import { CSSProperties, ReactNode } from "react"
import { GridLayout } from "@ks/types";

type Props = {
  pad?:number,
  imageSrc?:string,
  color?:string,
  overlay?:string,
  col?:number,
  layout:GridLayout,
  children:ReactNode|ReactNode[],
  id?:string,
  styles?:CSSProperties,
  className?:string
  height?:string,
}

export function Section({
  pad = 1, 
  height,
  imageSrc, 
  color, 
  overlay,
  col,
  layout = '1_1',
  children,
  id,
  styles,
  className,
}:Props
) {
  //                                  gotta put a '_' in front because css no like numbers as class names
  const stylesArr = [
    'section', 
    `grid`, 
    `overlay`,
    [`_${layout}`], 
    className,
  ]
  // todo trying global instead of module
  // const stylesArr = [styles.section, styles[`grid_${layout}`] ]

  const inlineStyles = {
    ...styles,
    minheight: height,
    "--c-overlay": overlay, 
  } as CSSProperties

  if(imageSrc) Object.assign(inlineStyles, {background: `url(${imageSrc})`})
  if(color) Object.assign(inlineStyles, {backgroundColor: color,}) 

  return (
    <section 
      id={id}
      className={stylesArr.join(' ')}
      style={inlineStyles}
    >

      {Array.isArray(children) ? children?.map((child:any, i:number) => (
        <div key={i}> 
          {child} 
        </div>
      )) : (
        <div>
          {children}
        </div>
      ) }
    
    </section>
  )
}

```

## Style

```scss
.overlay{
  position: relative;
  
  &::before {
    content: "";
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    background: var(--c-overlay);
  }
}
```

---
## Credits
- [css - how to create inline style with :before and :after - Stack Overflow](https://stackoverflow.com/questions/14436155/how-to-create-inline-style-with-before-and-after)
## Backlinks
- [[developer/ReactJS/ReactJS\|ReactJS]]