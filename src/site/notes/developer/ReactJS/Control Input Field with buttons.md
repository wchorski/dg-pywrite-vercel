---
{"dg-publish":true,"tags":["reactjs","nodejs","JSX"],"permalink":"/developer/ReactJS/Control Input Field with buttons/","dgPassFrontmatter":true}
---

With [[developer/ReactJS/ReactJS\|ReactJS]], I wanted to create an number input field that can both be edited by keyboard input and incremental buttons like

<input type='number' value=3 /> <button> + </button> <button> - </button>

Most tutorials do not utilize the `useRef` hook which I think helps simply and easily expand logic.

```jsx
import {useState, useEffect, useRef} from 'react'

import { TiArrowLoop } from 'react-icons/ti'
import { FaForward } from 'react-icons/fa'
import { IoIosTimer } from 'react-icons/io'

import { StyledPlayerSettings } from 'styles/PlayerSettings.styled'
import { GiMetronome } from 'react-icons/gi'
import { ToggleSwitch } from './ToggleSwitch'
import next from 'next'


export const Metronome = ({settings}) => {

  const inputSpeedRef = useRef(null);

  const [timerSpeed, setTimerSpeed] = useState(settings.advSpeed)
  const [metronomeToggle, setMetronomeToggle] = useState(false)
  const [counter, setCounter] = useState(0)
  const [activeSlides, setactiveSlides]     = useState([
    {id: 1, title: 'slide one', color: '#8dc3bf'},
    {id: 2, title: 'slide two', color: '#7aa19e'}, 
    {id: 3, title: 'slide three', color: '#4c6261'}
  ]);


  async function updateAdvSpeed(speed){
    const min = 1
    const max = 100
    const value = Math.max(min, Math.min(max, speed));
    setTimerSpeed(value)
    
    // set_database
  }


  useEffect(() => {

    const timer = setInterval(() => { 
      setCounter(count => {

          setMetronomeToggle( prev => !prev);
          if (count === activeSlides.length)  return 1
          return count += 1;
      });
    }, 1 * 1000);

    return () => {              
        clearInterval(timer);  
    };  
  // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [])
  


  return (

    <StyledPlayerSettings>
      <h2>Player Settings</h2>
      <label htmlFor="">change and preview slide advance tempo</label>

      {settings && 

        <article className='settings'>


            <div className="form-item timer">
              <IoIosTimer className='iconTimer'/>

              <input 
                ref={inputSpeedRef}
                type="number" 
                // defaultValue={settingsState[0].advSpeed}
                value={timerSpeed}
                min='1'
                max='100'
                
                onChange={(e) => updateAdvSpeed(e.target.value)}
              />

              <span 
                className='input-button add'  
                onClick={e => updateAdvSpeed(Number(inputSpeedRef.current.value) + 1)}   
              > + 
              </span>
              <span 
                className='input-button remove' 
                onClick={e => updateAdvSpeed(Number(inputSpeedRef.current.value) + -1)}   
              > - 
              </span>
            </div>
            <div className="preview">
              {metronomeToggle && (
                  <div className="metronome">
                    <GiMetronome />
                  </div>
                )}
                {!metronomeToggle && (
                  <div className="metronome">
                    <GiMetronome style={{transform: "scaleX(-1)"}} />
                  </div>  
                )}

                <ul className="slid-list">
                  {
                    activeSlides
                      .map((slide ) => (
                        <li className={slide.id === counter ? 'slide active' : 'slide'} key={slide.id} style={{backgroundColor: slide.color}}>

                          { slide.id === counter && (
                            <p>{slide.title}</p>
                          )}
                          
                        </li>
                      ))
                  }
                </ul>
            </div>
          }

        </article>
      }
    </StyledPlayerSettings>
  )
}

```

---
## Credits
- [Create numeric input with Min and Max validation in React | bobbyhadz](https://bobbyhadz.com/blog/react-number-input-min-max)
- [javascript - How to let the user input number and increase it with a button in React JS - Stack Overflow](https://stackoverflow.com/questions/61530157/how-to-let-the-user-input-number-and-increase-it-with-a-button-in-react-js)
