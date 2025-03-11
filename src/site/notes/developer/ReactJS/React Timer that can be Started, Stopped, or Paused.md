---
{"dg-publish":true,"tags":["reactjs","javascript","JSX"],"permalink":"/developer/ReactJS/React Timer that can be Started, Stopped, or Paused/","dgPassFrontmatter":true}
---

I wanted to create a `Game State` machine (with enum) that controlled a countdown timer. I was running in circles trying to figure out what was missing. I overlooked adding the state as a dependency in the `useEffect` hook 🙄

```jsx
import {useState, useEffect, useRef} from 'react'

export const TypingGame = ({ quote, content, getQuote }) => {

  const GAMESTATE = {SETUP: 'setup', PLAY: 'play', END: 'end'}

  const textareaRef = useRef(null)
  const contentRef  = useRef(null)
  const [countInit, setcountInit] = useState(10)
  const [countdown, setCountdown]   = useState(countInit)
  const [countSpeed, setCountSpeed] = useState(1)
  const [gameState, setGameState]   = useState(GAMESTATE.SETUP) // setup, play, end

  const [metronomeToggle, setMetronomeToggle] = useState(false)
  const [wpm, setWpm]               = useState(0)


  function textOnChange(val) {
    const spanArr = contentRef.current.querySelectorAll('span')
    const valArr = val.split('')

    let isDone = true

    spanArr.forEach((charSpan, index) => {
      const character = valArr[index]

      if (character == null) {
        charSpan.classList.remove('correct')
        charSpan.classList.remove('wrong')
        isDone = false

      } else if (character === charSpan.innerText) {
        charSpan.classList.add('correct')
        charSpan.classList.remove('wrong')

      } else {
        charSpan.classList.remove('correct')
        charSpan.classList.add('wrong')
        isDone = false

      }
    })
  
    if (isDone) handleGameOver() 
  }

  function handleGameOver() {
    // setIsFinished(true)
    setGameState(GAMESTATE.END)
    
  }

  function handleReset() {
    getQuote()
    setCountdown(countInit)
    textareaRef.current.value = ''
    const spanArr = contentRef.current.querySelectorAll('span')
    spanArr.forEach((charSpan, index) => {
        charSpan.classList.remove('correct')
        charSpan.classList.remove('wrong')
    })

    setGameState(GAMESTATE.SETUP)
  }

  async function handlePlay() {
    setGameState(GAMESTATE.PLAY)
    textareaRef.current.focus()
    console.log('play');
  }

  useEffect(() => {

    if(gameState === GAMESTATE.PLAY) {

      const timer = setInterval(() => { 
        setCountdown(count => {
          if (count === 0)  return handleGameOver()
          return count -= 1;
        });
          
      }, countSpeed * 1000);
    
      const metronome = setInterval(() => { 
        setMetronomeToggle( prev => !prev);
          
      }, 1 * 1000);

      return () => {clearInterval(timer); clearInterval(metronome)}
    }

  }, [countSpeed, gameState])

  //TODO credit: https://stackoverflow.com/questions/67242065/react-focus-on-an-input-element-that-have-input-with-disabled-property
  useEffect(() => {
    if (gameState === 'play') {
      textareaRef.current.focus();
    }
  }, [gameState]);

  return (<>

    <h1>Game State: {gameState}</h1>

    <div className="hud">
      <div className="countdown">
        countdown: {countdown}
        <span> {metronomeToggle ? <p>⏳</p> : <p>⌛️</p>} </span>
      </div>

      <div className="words-per-min">
        wpm: {wpm}
      </div>
    </div>    

    <article className='quote-cont'>
      {/* <p className='quote' ref={quoteRef}>{quote.content}</p> */}
      <q ref={contentRef} className='content'>{content.map(span => span)}</q>

      <cite className="quote-meta">
        <span className='author'>~{quote.author}</span>
        <span className='length'>| {quote.length}</span>
      </cite>
    </article>

    <div className={gameState === GAMESTATE.END ? 'end' : 'end transparent'}>
      <strong>Round End</strong>
      <br/>
      <button onPointerDown={e => handleReset()}>Play again?</button>
    </div>


    {gameState === GAMESTATE.SETUP && (
      // did not work with onPointerDown !!!! 
      <button onClick={handlePlay}> Play ▶️ </button>
    )}

    {/* <input ref={textareaRef} /> */}

    {/* hack: showing a blank input because focus input doesn't work in the same command line */}



      <textarea
        className={'playerinput'}
        ref={textareaRef} 
        onBlur={() => textareaRef.current.setSelectionRange(0, 0)}
        onChange={e => textOnChange(e.target.value)}
        disabled={gameState === GAMESTATE.END ? true : false}
      />

  </>)
}
```

---
## Credits
- [link](https://upmostly.com/tutorials/setinterval-in-react-components-using-hooks#:~:text=To stop an interval%2C you can use the,the React component unmounts the interval is cleared%3A)
- [[developer/Home Lab/ReactJS\|ReactJS]]
