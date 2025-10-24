

# Solution Code - Coin Flip Simulator

<details>
<summary> Step 1 Solution - Simulate a Single Coin Flip</summary>
JavaScript - script.js

```jsx
// ----- Select DOM elements -----
const flipButton = document.getElementById('flipButton');
const resultText = document.getElementById('resultText');

// ----- Single coin flip function -----
function calculateFlip() {
  const flip = Math.floor(Math.random() * 2);

  let result;
  if (flip === 0) {
    result = 'Heads';
  } else {
    result = 'Tails';
  }

  // Show the latest result on the page
  resultText.textContent = result;

}

// ----- Event listener -----
flipButton.addEventListener('click', calculateFlip);
```


</details>

<details>
<summary>Step 2 Solution – Track Results over Time</summary>
JavaScript - script.js

```jsx
const headsCountEl = document.getElementById('headsCount');
const tailsCountEl = document.getElementById('tailsCount');

// ----- Global state for totals -----
let heads = 0;
let tails = 0;

// ----- Single coin flip function (updates totals) -----
function calculateFlip() {
  const flip = Math.floor(Math.random() * 2);

  let result;
  if (flip === 0) {
    result = 'Heads';
  } else {
    result = 'Tails';
  }

  resultText.textContent = result;

  if (result === 'Heads') {
    heads++;
  } else {
    tails++;
  }

  headsCountEl.textContent = heads.toString();
  tailsCountEl.textContent = tails.toString();

}
```

</details>  
<details>
<summary>Step 3 Solution – Show a Running Percentage</summary>

JavaScript - script.js

```jsx
// ----- Select DOM elements -----
const headsPercentEl = document.getElementById('headsPercent');

// ----- Global state for totals -----
let heads = 0;
let tails = 0;

// ----- Single coin flip function (totals + percentage) -----
function calculateFlip() {

  // ... previous code
  
  const total = heads + tails;
  if (total > 0) {
    const percent = Math.round((heads / total) * 100);
    headsPercentEl.textContent = percent.toString() + '%';
  } else {
    headsPercentEl.textContent = '0%';
  }

}

// ----- Event listener -----
flipButton.addEventListener('click', calculateFlip);
```

</details>   
<details>

<summary> Step 4 Solution – Provide Support for Multiple Flips</summary>

HTML - index.html (update)

```html
<label for="flipCount">Number of flips:</label>
<input id="flipCount" type="number" min="1" value="1" />
```

JavaScript - script.js

```jsx
const flipCountInput = document.getElementById('flipCount');

// Previous code 

// ----- New batch function (skeleton only) -----
function performFlips() {
  const numFlips = Number(flipCountInput.value);

  for (let i = 0; i < numFlips; i++) {
    // Flip logic goes here (to be added in Step 5)
    console.log('Testing Flip')
  }
}

// ----- Event listener (still single flip for now) -----
flipButton.addEventListener('click', performFlips);
```

</details>   



<details>
<summary> Step 5 Solution – Migrate DOM Updates and Update calculateFlip()</summary>

JavaScript - script.js

```jsx
// ----- Refactored single flip function (pure: returns result only) -----
function calculateFlip() {
  const flip = Math.floor(Math.random() * 2);

  let result;
  if (flip === 0) {
    result = 'Heads';
  } else {
    result = 'Tails';
  }

  return result;
}

// ----- Batch function (loop, counters, lastResult, single DOM update) -----
function performFlips() {
  const numFlips = Number(flipCountInput.value);

  let lastResult = '';

  for (let i = 0; i < numFlips; i++) {
    const result = calculateFlip();
    lastResult = result;

    if (result === 'Heads') {
      heads++;
    } else {
      tails++;
    }
  }

  // Update DOM once after the loop
  resultText.textContent = lastResult;
  headsCountEl.textContent = heads.toString();
  tailsCountEl.textContent = tails.toString();

  const total = heads + tails;
  if (total > 0) {
    const percent = Math.round((heads / total) * 100);
    headsPercentEl.textContent = percent.toString() + '%';
  } else {
    headsPercentEl.textContent = '0%';
  }
}
```
</details>   



<details>

<summary> Step 6 Solution – Track Each Individual Result</summary>

HTML - index.html

Note: in `index.html`, add additional HTML:

```jsx
<p>History: <span id="flipHistory"></span></p>
```

JavaScript - script.js

Note: update `script.js`:

```jsx
// ----- Select DOM elements -----
const flipButton = document.getElementById('flipButton');
const resultText = document.getElementById('resultText');
const headsCountEl = document.getElementById('headsCount');
const tailsCountEl = document.getElementById('tailsCount');
const headsPercentEl = document.getElementById('headsPercent');
const flipCountInput = document.getElementById('flipCount');
const flipHistoryEl = document.getElementById('flipHistory');

// ----- Global state for totals + history -----
let heads = 0;
let tails = 0;
let flipHistory = [];

// ----- Refactored single flip function (pure) -----
function calculateFlip() {
  const flip = Math.floor(Math.random() * 2);

  let result;
  if (flip === 0) {
    result = 'Heads';
  } else {
    result = 'Tails';
  }

  return result;
}

// ----- Batch function with history -----
function performFlips() {
  const numFlips = Number(flipCountInput.value);

  let lastResult = '';

  for (let i = 0; i < numFlips; i++) {
    const result = calculateFlip();
    lastResult = result;

    if (result === 'Heads') {
      heads++;
    } else {
      tails++;
    }

    flipHistory.push(result);
  }

  // Update DOM once after the loop
  resultText.textContent = lastResult;
  headsCountEl.textContent = heads.toString();
  tailsCountEl.textContent = tails.toString();

  const total = heads + tails;
  if (total > 0) {
    const percent = Math.round((heads / total) * 100);
    headsPercentEl.textContent = percent.toString() + '%';
  } else {
    headsPercentEl.textContent = '0%';
  }

  // Show full flip history
  flipHistoryEl.textContent = flipHistory.join(', ');
}

// ----- Event listener -----
flipButton.addEventListener('click', performFlips);
```

</details>   


<details>
<summary>Final Solution</summary>

JavaScript - script.js

```jsx
// ----- Select DOM elements -----
const flipButton = document.getElementById('flipButton');
const resultText = document.getElementById('resultText');
const headsCountEl = document.getElementById('headsCount');
const tailsCountEl = document.getElementById('tailsCount');
const headsPercentEl = document.getElementById('headsPercent');
const flipCountInput = document.getElementById('flipCount');
const flipHistoryEl = document.getElementById('flipHistory');

// ----- Global state for totals + history -----
let heads = 0;
let tails = 0;
let flipHistory = [];

// ----- Refactored single flip function (pure) -----
function calculateFlip() {
  const flip = Math.floor(Math.random() * 2);

  let result;
  if (flip === 0) {
    result = 'Heads';
  } else {
    result = 'Tails';
  }

  return result;
}

// ----- Batch function with history -----
function performFlips() {
  const numFlips = Number(flipCountInput.value);

  let lastResult = '';

  for (let i = 0; i < numFlips; i++) {
    const result = calculateFlip();
    lastResult = result;

    if (result === 'Heads') {
      heads++;
    } else {
      tails++;
    }

    flipHistory.push(result);
  }

  // Update DOM once after the loop
  resultText.textContent = lastResult;
  headsCountEl.textContent = heads.toString();
  tailsCountEl.textContent = tails.toString();

  const total = heads + tails;
  if (total > 0) {
    const percent = Math.round((heads / total) * 100);
    headsPercentEl.textContent = percent.toString() + '%';
  } else {
    headsPercentEl.textContent = '0%';
  }

  // Show full flip history
  flipHistoryEl.textContent = flipHistory.join(', ');
}

// ----- Event listener -----
flipButton.addEventListener('click', performFlips);

```

HTML - index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Coin Flip Simulator</title>

  <!-- Base and custom styling -->
  <link rel="stylesheet" href="theme.css" />
  <link rel="stylesheet" href="styles.css" />
  
  <!-- Main logic file -->
  <script src="script.js" defer></script>
</head>
<body>
  <main class="container card">
    <h1>Coin Flip Simulator</h1>

    <!-- Batch flip input -->
    <label for="flipCount">Number of flips:</label>
    <input id="flipCount" type="number" min="1" value="1" />

    <!-- Button to trigger flip -->
    <button id="flipButton" class="btn btn-primary">Flip Coin</button>

    <!-- Result of the last flip -->
    <p id="resultText">Click the button to flip the coin.</p>

    <!-- Tally and percentage results -->
    <div id="totals">
      <p>Heads: <span id="headsCount">0</span></p>
      <p>Tails: <span id="tailsCount">0</span></p>
      <p>Heads Percentage: <span id="headsPercent">0%</span></p>
    </div>

    <!-- Flip history section -->
    <div id="history">
      <p>History: <span id="flipHistory">—</span></p>
    </div>
  </main>
</body>
</html>

```
</details>