# javascript-gotchas
Matt's list of javascript 'gotchas' to look out for when debugging

- Be sure you're not comparing a falsy value.
  - For example:
    ```js
    if (!someValue) // bad
    if (someValue === undefined) // good
    ```
  - It's not always necessary to be this explicit, but hey, you're only reading this because you're stuck on a bug, why not give this a try.
