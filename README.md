# javascript-gotchas
Matt's list of javascript 'gotchas' to look out for when debugging

- Be sure you're not comparing a falsy value.
  - For example:
    ```js
    if (!someValue) // bad
    if (someValue === undefined) // good
    ```
  - It's not always necessary to be this explicit, but hey, you're only reading this because you're stuck on a bug, why not give this a try.

- Be sure you're not mutating an object that should not be mutated (aka is this a pure function?)
  - For example:
     ```js
     const defaultArray = [1, 2, 3];
     const arrayWeWant = defaultArray;
     if (someCondition) {
       arrayWeWant.push(somethingElse); // NOPE! you just mutated the value defaultArray
     }
     
     // Instead
     const defaultArray = [1, 2, 3];
     const arrayWeWant = defaultArray.slice(); // assign the variable to a copy of the original value
     if (someCondition) {
       arrayWeWant.push(somethingElse); // 😎
     }
     ```

- Be careful if blending comparators (`&&`, `||`) and ternary. You need to wrap the ternary in parenthesis
  - For example:
    ```js
      console.log((true && 'foo') || 'a' === 'a' ? 'a' : 'b') // 'a'
      console.log((true && 'foo') || 'a' === 'b' ? 'a' : 'b') // 'a' wtf?
      console.log((true && 'foo') || ('a' === 'b' ? 'a' : 'b')) // 'foo' that's more like what we were expecting
      console.log((false && 'foo') || ('a' === 'b' ? 'a' : 'b')) // 'b' much better
   ```
