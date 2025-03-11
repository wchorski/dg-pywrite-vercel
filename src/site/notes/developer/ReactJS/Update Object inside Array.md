---
{"dg-publish":true,"permalink":"/developer/ReactJS/Update Object inside Array/","dgPassFrontmatter":true}
---

[[developer/Home Lab/ReactJS\|ReactJS]]

wanted to update a single object inside an array of 

```jsx
import {useState} from 'react';

const App = () => {
  const initialState = [
    {id: 1, country: 'Austria'},
    {id: 2, country: 'Belgium'},
    {id: 3, country: 'Canada'},
  ];

  const [data, setData] = useState(initialState);

  const updateState = () => {
    // 👇️ passing function to setData method
    setData(prevState => {
      const newState = prevState.map(obj => {
        // 👇️ if id equals 2, update country property
        if (obj.id === 2) {
          return {...obj, country: 'Denmark'};
        }

        // 👇️ otherwise return object as is
        return obj;
      });

      return newState;
    });
  };

  return (
    <div>
      <button onClick={updateState}>Update state</button>

      {data.map(obj => {
        return (
          <div key={obj.id}>
            <h2>id: {obj.id}</h2>
            <h2>country: {obj.country}</h2>
            <hr />
          </div>
        );
      })}
    </div>
  );
};

export default App;
```

---
## Credits
- [link](https://bobbyhadz.com/blog/react-update-object-in-array#:~:text=To update an object in an array in,condition and return all other objects as is.)

## Backlinks
- [[developer/ReactJS/ReactJS\|ReactJS]]