
# ReactJS

Basics:
-
1. Creating an Application with typescript
    npx create-react-app typescript-tutorial --template typescript

# Best Practice



Coding :
-           

using primitives,array,objects,list,map
```
import React from 'react';
import './App.css';

//primitives
let a: string = "renga";
let b: number = 32;
let flag: boolean = false;
let strArr: string[] = ["r", "e", "n"];
let tuple: [string, number] = ["renga", 43];

//objects
type Person = {
  name: string;
  age?: number;
}

let personInstance1: Person = {
  name: "renga",
  age: 38,
};

let personInstance2: Person = {
  name: "samy",
  age: 38,
};

//object array
let personLst: Person[] = [];

personLst[0] = personInstance1;
//personLst[1] = personInstance2;

//map 
let personMap: Map<string, string> = new Map<string, string>();



personMap.set("name", personInstance1.name);
personMap.set(
  "age",
  personInstance1.age === undefined ? "32" : personInstance1.age.toString()
);

function App() {
  return (
    <div>
      {personLst.map((item) => (
        <div>
          {item.name}
          {item.age}
          {a}
          {b}
          {flag}
          {strArr}
          {personInstance1.name}
          {tuple}
        </div>
      ))}
      {personMap}
    </div>
  );
}

export default App;

```
 
Jargons :
-
npm
npx
babel
webpack
tsc
transpiling
eslint
prettier
tsconfig
package.json
