---
{"dg-publish":true,"permalink":"/developer/react-js/react-adsr-envelope-ui-with-svg/","dgPassFrontmatter":true}
---

There's probably a better way to build this with `html <canvas>` but I'm opting to build this out with `<intput type="range"` and `<svg>` and [[developer/ReactJS/ReactJS\|ReactJS]]. 

> [!warning] This is a draft
> What you see here is an unfinished component. This post will be updated as I build out this component more.

## Pre Planning

The `useReducer` hook will be perfect for this as I'll need to reference and update many nested objects in multiple places. 

## Component

Here is a start of my component

```tsx
import React, { useReducer, useState } from "react";
import styles from '../styles/env.module.scss'

type ValueType = 'attack'|'decay'|'sustain'|'release'

type Props = {
  prop?:string
}

type EnvPoint = {
  time:number,
  x:number,
  y:number,
}

type EnvState = {
  attack:EnvPoint,
  decay:EnvPoint,
  sustain:EnvPoint,
  release:EnvPoint,
}

type Action =
  | { type: 'RESET' }
  | { type: 'SET_VALUE'; payload: {
    time:number, 
    type:ValueType,
    x:number,
    y:number,
  }}

const initialState:EnvState = {
  attack: {
    time: 10,
    x: 0,
    y: 10,
  },
  decay: {
    time: 20,
    x: 0,
    y: 50,
  },
  sustain: {
    time: 50,
    x: 0,
    y: 50,
  },
  release: {
    time: 10,
    x: 0,
    y: 90,
  },
};

const reducer = (state:EnvState, action:Action) => {
  switch (action.type) {

    case "SET_VALUE":
      return {
        ...state,
        [action.payload.type]: { 
          time: action.payload.time,
          x: action.payload.x,
          y: action.payload.y
        }
      }

    case "RESET":
      return initialState

    default:
      return state;
  }
};


export function EnvelopeLFOEditor ({ prop }:Props) {

  const [state, dispatch] = useReducer(reducer, initialState);

  return <div>

    <svg className={styles.env} width="100" height="100">

      <line 
        x1={0}  y1={100} 
        x2={state.attack.x} y2={state.attack.y} 
        stroke="green" 
      />
      
      <circle 
        cx={state.attack.x} 
        cy="10" r="8" 
        stroke="green" strokeWidth="2" fill="lime" 
      /> 

      <line 
        x1={state.attack.x}  y1={state.attack.y} 
        x2={state.decay.x} y2={state.decay.y} 
        stroke="green" 
      />

      <circle 
        cx={state.decay.x} 
        cy={state.decay.y} 
        r="8" 
        stroke="green" strokeWidth="2" fill="lime"
      />

      <line 
        x1={state.decay.x}  y1={state.decay.y} 
        x2={state.sustain.x} y2={state.sustain.y} 
        stroke="green" 
      />

      <circle 
        cx={state.sustain.x} 
        cy="50" 
        r="8" 
        stroke="green" strokeWidth="2" fill="lime"
      />

      <line 
        x1={state.sustain.x} y1={state.sustain.y} 
        x2={state.release.x}  y2={state.release.y} 
        stroke="green" 
      />

      <circle 
        cx={state.release.x} 
        cy="90" 
        r="8" 
        stroke="green" strokeWidth="2" fill="lime"
      />
    </svg>
  
    <div className={styles.controls}>
      <label htmlFor="attack" className={styles.slider} >
        <span> attack: {state.attack.time} </span>
        <input 
          name="attack" id="attack" 
          type="range" min="0" max="100" 
          value={state.attack.time}
          onChange={(e) => dispatch({type: 'SET_VALUE', 
            payload: { 
              type: 'attack', 
              time: Number(e.target.value),
              x: Number(e.target.value),
              y: state.attack.y,
            }
          })}
        />
      </label>
      <label htmlFor="decay" className={styles.slider} >
        <span> decay: {state.decay.time}  </span>
        <input 
          name="decay" id="decay" 
          type="range" min="0" max="100" 
          value={state.decay.time}
          onChange={(e) => dispatch({type: 'SET_VALUE', 
            payload: { 
              type: 'decay', 
              time: Number(e.target.value),
              x: Number(e.target.value) + state.attack.time,
              y: state.decay.y,
            }
          })}
        />
      </label>
      <label htmlFor="sustain" className={styles.slider} >
        <span> sustain: {state.sustain.time}  </span>
        <input 
          name="sustain" id="sustain" 
          type="range" min="0" max="100" 
          value={state.sustain.time}
          onChange={(e) => dispatch({type: 'SET_VALUE', 
            payload: { 
              type: 'sustain', 
              time: Number(e.target.value),
              x: Number(e.target.value) + (state.attack.time + state.decay.time),
              y: state.sustain.y,
            }
          })}
        />
      </label>
      <label htmlFor="release" className={styles.slider} >
        <span> release: {state.release.time}  </span>
        <input 
          name="release" id="release" 
          type="range" min="0" max="100" 
          value={state.release.time}
          onChange={(e) => dispatch({type: 'SET_VALUE', 
            payload: { 
              type: 'release', 
              time: Number(e.target.value),
              x: Number(e.target.value) + (state.attack.time + state.decay.time + state.sustain.time),
              y: state.release.y,
            }
          })}
        />
      </label>
    </div>

    <h4> state obj </h4>
    {JSON.stringify(state, null, 2)}

  </div>
}
```

[[developer/ReactJS/ReactJS\|ReactJS]]