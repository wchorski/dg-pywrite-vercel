---
{"dg-publish":true,"permalink":"/developer/ReactJS/React One Shot Animation with CSS Trick/","dgPassFrontmatter":true}
---


simple, basic
```tsx
import styled from "styled-components"
import { MdShoppingBag } from "react-icons/md";


export function CartCount2({ count }: { count: number }) {

  return (
    <StyledCartIcon>

      <StyledDot key={count}>
        <span> {count} </span>
      </StyledDot>

      <MdShoppingBag />

    </StyledCartIcon>
  )
}


const StyledDot = styled.div`

  background-color: var(--c-txt-rev);
  color: var(--c-txt);
  padding: 0.2rem;
  position: absolute;
  top: -15px;
  right: 0;
  border-radius: 100px;
  line-height: 2rem;
  min-width: 2.5rem;
  font-feature-settings: 'tnum';
  font-variant-numeric: tabular-nums;
  animation: bigSmall .3s;
  animation-timing-function: cubic-bezier(0.4, 0, 0.2, 1);


  @keyframes bigSmall {
    0% { 
      font-size: 25px;
      padding: 1rem;
    }
    100% { 
      font-size: 2px;
      padding: 0.2rem;
    }
  }

  span {

    transition: all .3s;
  }
`

const StyledCartIcon = styled.div`
  position: relative;
`
```


---
## Credits
- [[developer/ReactJS/ReactJS\|ReactJS]]
- [reactjs - How to trigger a CSS animation on EVERY TIME a react component re-renders - Stack Overflow](https://stackoverflow.com/questions/63186710/how-to-trigger-a-css-animation-on-every-time-a-react-component-re-renders)