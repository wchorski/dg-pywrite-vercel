---
{"dg-publish":true,"permalink":"/developer/react-js/no-library-needed-for-css-transitions/","dgPassFrontmatter":true}
---

## Custom Hook
`useStyleTransition.tsx`
```tsx
// @ts-nocheck
import { useEffect, useState } from "react"

const STATE = {
  ENTERING: 'ent',
  ENTERED: 'entd',
  EXITING: 'exit',
  EXITED: 'exitd',
}

function useTransitionState(duration = 1000) {

	const inSeconds = duration * 1000

  const [state, setState] = useState<string>('')

  useEffect(() => {
    let timerId: any

    if (state === STATE.ENTERING) {
      timerId = setTimeout(() => setState(STATE.ENTERED), inSeconds)
    } else if (state === STATE.EXITING) {
      timerId = setTimeout(() => setState(STATE.EXITED), inSeconds)
    }

    return () => timerId && clearTimeout(timerId)

  }, [state, setState])


  return [state, setState]
}

export function useStyleTransitionControl(seconds: number) {
  const [state, setState] = useTransitionState(seconds)

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
`EmojiExample.tsx`
```tsx
// @ts-nocheck
import styled from "styled-components"
import { useStyleTransitionControl } from "../../lib/useStyleTransition"

export function EmojiFade() {

  const [state, enter, exit] = useStyleTransitionControl(1)

  return (
    <StyledEmoji >
      State: {state}

      <div className={`emoji-cont ${state}`}>
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

const StyledEmoji = styled.div`
  .emoji-cont{

    transition: all .3s ease-in-out;

    &.ent{ 
      opacity: 1;
    }
    &.entd{ 
      opacity: 1;
    }
    &.exit{ 
      opacity: 0;
    }
    &.exitd{ 
      opacity: 0;
    }

  }
`
```
> [!warning] I'm ignoring all typescript errors at the top of each file. Still trying to figure that out

---
## Credits 
- [Applying CSS Transitions with React Hooks | by Kaylie Kwon | JavaScript in Plain English](https://javascript.plainenglish.io/applying-css-transitions-with-react-hooks-7bd84671bc6b)
- cred Kaylie Kwon - https://javascript.plainenglish.io/applying-css-transitions-with-react-hooks-7bd84671bc6b
## Backlinks
- [[developer/ReactJS/ReactJS\|ReactJS]]
- [[developer/CSS/CSS\|CSS]]
