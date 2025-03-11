---
{"dg-publish":true,"permalink":"/developer/Javascript/Find and Remove Multiple Objects from an Array/","dgPassFrontmatter":true}
---

putting a `Array.splice()` function inside of a loop can make things pretty confusing. Here is the steps of logic that got me to this function below
1. start with an array of objects
2. create an String Array that keeps my selectors for removal
3. loop through the **object array** against the **string array** to find relative indexes
4. loop through the new **index array** backwards as we mutate the **object array**

```js
let students = [
    { name: 'John', age: 24 },
    { name: 'Peter', age: 20 },
    { name: 'Mary', age: 19 },
    { name: 'Bill', age: 22 }
];

const rmNames = ["John", "Bill"]
const rmIndex = [];

let filteredArr = []

students.map(s => {
    if(rmNames.includes(s.name)){
        let found = students.find(item => item.name === s.name);
        rmIndex.push(students.indexOf(found))
    }
})

console.log("rmIndex, ", rmIndex)

for (var i = rmIndex.length - 1; i >= 0; i--){
    students.splice(rmIndex[i], 1);
}


console.log('students ===', students);

// output → 
// students === [{"name":"Peter","age":20},{"name":"Mary","age":19}]
```

---
## Credits
- [javascript - Remove items from array with splice in for loop - Stack Overflow](https://stackoverflow.com/a/16217435/15579591)