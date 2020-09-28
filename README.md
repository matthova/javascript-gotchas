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

- Don't try to be clever when making a multidimensional array
  - If you want a 2 x 3 array and you you do this
    ```js
    const myArray = new Array(2).fill(new Array(3).fill(0))
    myArray[0][0] = 1;
    ```
  - You will get this
    ```js
    [[1, 0, 0], [1, 0, 0]]
    ```
    because each row will contain a pointer to the same object `new Array(3).fill(0)`.
 
  - Instead do this
    ```js
    const rows = 2;
    const columns = 3;
    const myArray = new Array(rows);
    for(let y = 0; y < rows; y++) {
      myArray[y] = new Array(columns).fill(0);
    }
    ```
 
 - Are you trying to parse a json objet you pulled from a database? Make sure you've parsed it from a string into an object.
 
 - Did you recently expand an object that had two states into one with three states? Be sure you're not falling into this trap:
   ```js
     // before
     emum ConnectionStatus {
       Disconnected = 'disconnected',
       Connected = 'connected',
     }
     
     const doSomethingWithConnectionStatus = (status: ConnectionStatus) {
       if (status !== ConnectionStatus.Connected) {
         alert('Youre disconnected!');
       }
     }
     
     // after
     emum ConnectionStatus {
       Disconnected = 'disconnected',
       Connected = 'connected',
       ConnectedWithVideo = 'connectedWithVideo'
     }
     
     const doSomethingWithConnectionStatus = (status: ConnectionStatus) {
       if (status !== ConnectionStatus.Connected) {
         alert('Youre disconnected!');
         // WRONG! (if you're connected with video).
         // You need to make sure the added enum values did not affect any existing logic
       }
     }
  ```
