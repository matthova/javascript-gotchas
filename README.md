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

- Be careful if blending comparators (`&&`, `||`) and ternary. You need to wrap the ternary in parenthesis
  - For example:
    ```js
      (true && 'foo') || 'a' === 'a' ? 'a' : 'b') // 'a'
      (true && 'foo') || 'a' === 'b' ? 'a' : 'b') // 'a' wtf?
      (true && 'foo') || ('a' === 'b' ? 'a' : 'b')) // 'foo' that's more like what we were expecting
      (false && 'foo') || ('a' === 'b' ? 'a' : 'b')) // 'b' much better
   ```
