---
{"dg-publish":true,"permalink":"/developer/ReactJS/ReactJS Popup with Dialogue Modal/","dgPassFrontmatter":true}
---

Watching a [Web Dev Simplified Tutorial](https://www.youtube.com/@WebDevSimplified) I wanted to port this idea of using `<dialogue>` as a symantically correct popup component. Here is my port to #reactjs 

```tsx
import { useRef, useEffect, Dispatch, SetStateAction } from 'react'
import { RiCloseFill } from 'react-icons/ri'
import styled from 'styled-components'

type Props = {
  isShown: boolean,
  setIsShown: Dispatch<SetStateAction<boolean>>,
}

export default function TicketPopup({isShown, setIsShown}:Props) {

  const ticketPopupRef = useRef<HTMLDialogElement>(null)

  function handleOnClick(e:React.MouseEvent<HTMLDialogElement, MouseEvent>) {
    const { left, right, top, bottom } = e.currentTarget.getBoundingClientRect();
    if (
      e.clientX < left ||
      e.clientX > right ||
      e.clientY < top ||
      e.clientY > bottom
    ) {
      setIsShown(false)
    }
  }

  useEffect(() => {
    if(isShown) return ticketPopupRef.current?.showModal()
    if(!isShown) return ticketPopupRef.current?.close()
    // return () =>
  }, [isShown])
  
  
  return (
    <StyledPopup 
      ref={ticketPopupRef}
      onClick={handleOnClick}
    >
      <button onClick={e => setIsShown(false)} > 
        <RiCloseFill />
      </button>
      <h2> Purchase Tickets </h2>
    </StyledPopup>
  )
}


const StyledPopup = styled.dialog`

  transition: all 1s;

  &::backdrop{
    transition: all 1s;
    background-color: #00000094;
  }
`
```


> [!warning] Hot Reload Error
> "InvalidStateError: Failed to execute 'showModal' on 'HTMLDialogElement': The element already has an 'open' attribute, and therefore cannot be opened modally."
> 
> While in development, errors usually get thrown during a hot reload. That's because the component is reading multiple 'open'. I assume this can safely be ignored in production

I'm not a big fan of passing down an `useState` hook from parent to  child component, but it was the first idea I had.

---
## Credit
- Web Dev Simplified - https://www.youtube.com/watch?v=ywtkJkxJsdg&t=473s

## Backlinks
- [[developer/ReactJS/ReactJS\|ReactJS]]