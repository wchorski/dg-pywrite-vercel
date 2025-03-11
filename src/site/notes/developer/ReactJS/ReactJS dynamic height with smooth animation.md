---
{"dg-publish":true,"permalink":"/developer/ReactJS/ReactJS dynamic height with smooth animation/","dgPassFrontmatter":true}
---

If you've tried the `max-height` css trick to make smooth height animations, you'll realize that this isn't a one size fits all solution (literally)

## QuestionAnswer.tsx
```tsx
import { useEffect, useRef, useState } from "react"
import styled from "styled-components"

type iProps = {
  question: string,
  answer: string,
}

export function QuestionAnswer({question = 'I have a question!', answer = 'Here is your answer'}:iProps) {

  const [isShown, setisShown] = useState(false)
  const scrollContRef = useRef<HTMLParagraphElement>(null)


  function handleTextFormat(text:string){
    const array = text.split('\n')
    return array
  }

  
  return (
    <StyledAnswer scrollHeight={ scrollContRef.current ? scrollContRef.current.scrollHeight + 100 : 100}>

      <h4>Question: </h4>

      <p className="question"> 
        {handleTextFormat(question).map((line:string, i:number) => (
          <p key={i}>{line}</p>
        ))}
      </p>

      <h4>Answer: </h4>

      <button onClick={() => setisShown(!isShown)}>
        {isShown ? 'hide' : 'reveal'}
      </button>

      <div ref={scrollContRef} className={isShown ? 'expanded answer' : 'collapsed answer'}>
        {handleTextFormat(answer).map((line:string, i:number) => (
          <p key={i}>{line}</p>
        ))}
      </div>

    </StyledAnswer>
  )
}

const StyledAnswer = styled.div<{scrollHeight:number}>`

  border: solid black 1px;
  padding: 1em;
  margin: 1em 0;

  h4{
    margin-bottom: 0;
  }

  div.quetion{

  }

  div.answer{
    border: solid 1px black;
    border-radius: 5px;

    font-size: 16px;
    line-height: 38px;
    padding: 0.3em;
    overflow-y: hidden;
    transition: all 1s ease-in-out;
  }

  .collapsed{
    max-height: 0;
    background-color: #05052d;
  }

  .expanded{
    background-color: white;
    max-height: ${props => props.scrollHeight || 30}px;
  }

`
```

I also throw in a bonus line break function that reads any `\n` and throws in a `<br/>`


## App.tsx
```tsx

import {QuestionAnswer} from './QuestionAnswer
'

export default function App() {
  return (
    <div className="App">
      
      <QuestionAnswer 
        question={`Sometimes I like to put the punchline first`}
        answer={`A lot of people asked me why the line for drinks is before the line for food, so I explained`}
      />
      <QuestionAnswer 
        question={`My wife told me to stop acting like a flamingo.`}
        answer={`I had to put my foot down.`}
      />
      <QuestionAnswer 
        question={`Is your refrigerator running?`}
        answer={`Because if it is, you might want to catch it and tell it to slow down before it gets overworked and burns out, leaving you with a fridge full of spoiled food and a hefty repair bill. And if it's not running, you might want to check if it's even plugged in, or maybe it's just taking a well-deserved break after working tirelessly to keep your food fresh and cold.`}
      />
      <QuestionAnswer 
        question={`Did you hear about the new restaurant called Karma?`}
        answer={`Well, the concept is that you don't actually order any food - instead, the universe decides what you'll eat based on your past actions. So if you've been kind and generous throughout your life, you might get a delicious steak dinner with all the fixings. But if you've been a total jerk, you might end up with a moldy sandwich and a side of expired milk. Anyway, I decided to give it a try and see what I'd get. When I walked in, I was greeted by a wise-looking waiter who informed me that my meal would be determined by a complex algorithm that takes into account my entire life history. I was a little nervous, but also curious to see what I'd get. So I sat down at a table and waited for my fate to be revealed. \n \n

        As I waited, I couldn't help but think about all the good things I'd done in my life - I'd volunteered at a soup kitchen, I'd donated to charity, I'd even rescued a cat from a tree once. Surely, I thought, the universe would reward me with something truly delicious. But as the minutes ticked by, my confidence started to wane. Maybe I'd done some bad things too - I mean, who hasn't told a little white lie or two? And what about that time I stole a pack of gum from the corner store when I was a kid? Would that count against me? \n \n
        
        Finally, after what felt like an eternity, the waiter returned with my meal. He placed a small plate in front of me, and on it was a single pea. I looked up at him in disbelief. "A pea?" I asked. "That's it?" The waiter nodded gravely. "I'm sorry," he said. "But according to our algorithm, this is all you deserve." I was incredulous. "But how is that possible?" I asked. "I've done so many good things in my life!" The waiter sighed. "I'm afraid it's not just about the good things you've done," he said. "It's also about the bad things. And according to our records, you once spent an entire evening watching cat videos instead of finishing a work project that was due the next day. That, my friend, is why you only get a pea."`}
      />
    </div>
  );
}
```

You can see I use `answers` (in this case punchlines from jokes) with varying lengths to show that this dynamically reacts to the content's height and not just arbitrary large number.

---
## Credits
- [javascript - How do you put a line break in a React string? - Stack Overflow](https://stackoverflow.com/questions/55541031/how-do-you-put-a-line-break-in-a-react-string)
- [reactjs - React - styled-components, props, and TypeScript - Stack Overflow](https://stackoverflow.com/questions/60992002/react-styled-components-props-and-typescript)
- [link](https://www.webmound.com/get-scrollable-height-javascript/#:~:text=These are the two JavaScript properties that can,may be added by its border or margin.)
- [[developer/Home Lab/ReactJS\|ReactJS]]