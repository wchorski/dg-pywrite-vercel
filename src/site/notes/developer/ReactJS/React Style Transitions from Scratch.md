---
{"dg-publish":true,"permalink":"/developer/ReactJS/React Style Transitions from Scratch/","dgPassFrontmatter":true}
---

## Custom Hook
`lib/useStyleTransition.tsx`
```jsx
import { useEffect, useState } from "react"

const STATE = {
  ENTERING: 'entering',
  ENTERED: 'entered',
  EXITING: 'exiting',
  EXITED: 'exit',
}

function useTransitionState(duration = 1000) {

  const [state, setState] = useState<string>()

  useEffect(() => {
    let timerId: any

    if (state === STATE.ENTERING) {
      timerId = setTimeout(() => setState(STATE.ENTERED), duration)
    } else if (state === STATE.EXITING) {
      timerId = setTimeout(() => setState(STATE.EXITED), duration)
    }

    return () => timerId && clearTimeout(timerId)

  }, [state, setState])


  return [state, setState]
}

export function useStyleTransitionControl(duration: number) {
  const [state, setState] = useTransitionState(duration)

  const enter = () => {
    if (state !== STATE.EXITING) {
      setState(STATE.ENTERING)
    }
  }

  const exit = () => {
    if (state !== STATE.ENTERING) {
      setState(STATE.EXITING)
    }
  }

  return [state, enter, exit]
}
```

## Rendered Component
`EmojiFade.tsx`
```jsx
import { useStyleTransitionControl } from "../../lib/useStyleTransition"
import { useState } from "react"

const defaultStyle = {
  transition: 'opacity 3000ms ease-in-out',
  opacity: 0,
}

const transitionStyles = {
  entering: { opacity: 1 },
  entered: { opacity: 1 },
  exiting: { opacity: 0 },
  exited: { opacity: 0 },
}

export function EmojiFade() {

  const [state, enter, exit] = useStyleTransitionControl()
  const [isOpen, setIsOpen] = useState(false)

  const style = {
    ...defaultStyle,
    ...transitionStyles[state] ?? {}
  }

  return (
    <StyledEmoji open={isOpen}>
      State: {state}

      <div style={style} > 
        <span role="img" aria-label="fading emoji"> 🐸 </span>
      </div>

      <button onClick={enter}>
        Enter
      </button>

      <button onClick={exit}>
        Exit
      </button>

    </StyledEmoji>
  )
}


```

---
## Credits
- [Applying CSS Transitions with React Hooks | by Kaylie Kwon | JavaScript in Plain English](https://javascript.plainenglish.io/applying-css-transitions-with-react-hooks-7bd84671bc6b)

## Backlinks
- [[developer/ReactJS/ReactJS\|ReactJS]]

