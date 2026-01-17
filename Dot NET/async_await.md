# Understanding Async/Await in JavaScript

## What is Async/Await?

### `async`
Declares an asynchronous function, which means it returns a promise.

### `await`
Pauses the execution of the `async` function until the promise is resolved or rejected. (The rest of the code within the `async` function will not execute until the promise is resolved.)

---

## Example

```javascript
const fetchData = async () => {
  console.log('Fetching data...');

  // Pauses here until the fetch promise resolves
  const response = await fetch('https://jsonplaceholder.typicode.com/users');

  console.log('Data fetched!'); // This runs after the fetch is complete

  const data = await response.json(); // Pauses here until JSON parsing is complete
  console.log('Parsed data:', data); // Runs after JSON parsing is done
};

fetchData();
console.log('This runs while waiting for fetch to complete');
```

---

## Console Output

```
Fetching data...
This runs while waiting for fetch to complete
Data fetched!
Parsed data: [{...}, {...}, ...] // JSON data from the API
```

---

## Key Takeaways

1. The code **inside the `async` function after `await`** is paused until the promise resolves.
2. The **rest of the JavaScript application (outside the `async` function)** can continue running during the pause.
3. This behavior allows your app to remain responsive while waiting for asynchronous operations to complete.
