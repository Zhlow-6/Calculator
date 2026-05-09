<script>
  import { onMount } from "svelte";

  // ==================== VARIABLES ====================
  // ✅ WORKING: Reactive variables (demonstrates Svelte reactivity)
  let firstNumber = 0;
  let secondNumber = 0;
  let result = 0;
  let operation = ""; // Track which operation was used
  let historyList = []; // Store calculation history

  // ==================== FUNCTIONS ====================

  // ✅ WORKING: Addition function (demonstrates basic arithmetic operation)
  function addition() {
    result = (firstNumber + secondNumber).toFixed(2);
    operation = "Addition";
    historyList.unshift(`${firstNumber} + ${secondNumber} = ${result}`);
    historyList = historyList.slice(0, 5);
  }

  // ✅ WORKING: Multiplication function (demonstrates another operation pattern)
  function multiplication() {
    result = (firstNumber * secondNumber).toFixed(2);
    operation = "Multiplication";
    historyList.unshift(`${firstNumber} × ${secondNumber} = ${result}`);
    historyList = historyList.slice(0, 5);
  }

  // ✅ WORKING: Clear function (demonstrates state reset pattern)
  function clear() {
    firstNumber = 0;
    secondNumber = 0;
    result = 0;
    operation = "";
  }

  // Clear history function
  function clearHistory() {
    historyList = [];
  }

  // ⚠️ TODO: Implement subtraction function (Your Turn!)
  // SUCCESS CRITERIA:
  // - Subtract secondNumber from firstNumber
  // - Store result in result variable
  // - Set operation to 'Subtraction'
  // - Test with positive and negative numbers
  //
  // HINTS:
  // - Follow the addition() pattern above
  // - Use the - operator
  // - Example: result = firstNumber - secondNumber;
  function subtraction() {
    result = (firstNumber - secondNumber).toFixed(2);
    operation = "Subtraction";
    historyList.unshift(`${firstNumber} - ${secondNumber} = ${result}`);
    historyList = historyList.slice(0, 5);
  }

  // ⚠️ TODO: Implement division function with error handling (Your Turn!)
  // SUCCESS CRITERIA:
  // - Check if secondNumber is 0 before dividing
  // - Show "Error: Cannot divide by zero" if secondNumber is 0
  // - Otherwise calculate firstNumber / secondNumber
  // - Set operation to 'Division'
  //
  // HINTS:
  // - Use an if-else statement
  // - Check: if (secondNumber === 0)
  // - Set result to error message (string) or calculation (number)
  function division() {
    if (secondNumber === 0) {
      result = "Error: Cannot divide by zero";
    } else {
      result = (firstNumber / secondNumber).toFixed(2);
      historyList.unshift(`${firstNumber} ÷ ${secondNumber} = ${result}`);
      historyList = historyList.slice(0, 5);
    }
    operation = "Division";
  }

  // ⚠️ TODO: Implement modulo function (Your Turn!)
  // SUCCESS CRITERIA:
  // - Calculate remainder using % operator
  // - Store in result variable
  // - Set operation to 'Modulo'
  // - Test with various number combinations
  //
  // HINTS:
  // - Follow the multiplication() pattern
  // - Use the % operator for remainder
  // - Example: 17 % 5 = 2
  function modulo() {
    console.log("CLICKED", firstNumber, secondNumber);
    if (secondNumber === 0) {
      result = "Error: Cannot modulo by zero";
    } else {
      result = (Number(firstNumber) % Number(secondNumber)).toFixed(2);
      historyList.unshift(`${firstNumber} % ${secondNumber} = ${result}`);
      historyList = historyList.slice(0, 5);
    }
    operation = "Modulo";
  }

  // Keyboard support
  onMount(() => {
    const handleKeydown = (event) => {
      // Prevent keyboard shortcuts when typing in input fields
      if (event.target.tagName === "INPUT") return;

      if (event.key === "+") {
        event.preventDefault();
        addition();
      } else if (event.key === "-") {
        event.preventDefault();
        subtraction();
      } else if (event.key === "/") {
        event.preventDefault();
        division();
      } else if (event.key === "%") {
        event.preventDefault();
        modulo();
      } else if (event.key.toLowerCase() === "c") {
        event.preventDefault();
        clear();
      }
    };

    document.addEventListener("keydown", handleKeydown);

    return () => {
      document.removeEventListener("keydown", handleKeydown);
    };
  });
</script>

<div class="container mt-5">
  <div class="calculator-container">
    <h1 class="text-center mb-4">🧮 Math Calculator</h1>
    <p class="text-center text-muted small mb-4">
      Keyboard shortcuts: + (add), - (subtract), / (divide), % (modulo), C
      (clear)
    </p>

    <!-- ✅ WORKING: Input Fields with bind:value (demonstrates two-way binding) -->
    <div class="row mb-3">
      <div class="col-md-6">
        <input
          type="number"
          class="form-control"
          placeholder="First Number"
          bind:value={firstNumber}
        />
      </div>
      <div class="col-md-6">
        <input
          type="number"
          class="form-control"
          placeholder="Second Number"
          bind:value={secondNumber}
        />
      </div>
    </div>

    <!-- ✅ WORKING: Operation Buttons (2 working, 3 for students to connect) -->
    <div class="row mb-3">
      <div class="col">
        <!-- ✅ WORKING: Addition button with on:click -->
        <button class="btn btn-primary btn-block" on:click={addition}>
          + Add
        </button>
      </div>
      <div class="col">
        <!-- ⚠️ TODO: Add on:click handler for subtraction -->
        <button class="btn btn-secondary btn-block" on:click={subtraction}>
          - Subtract
        </button>
      </div>
      <div class="col">
        <!-- ✅ WORKING: Multiplication button with on:click -->
        <button class="btn btn-success btn-block" on:click={multiplication}>
          × Multiply
        </button>
      </div>
      <div class="col">
        <button class="btn btn-warning btn-block" on:click={division}>
          ÷ Divide
        </button>
      </div>
      <div class="col">
        <!-- ⚠️ TODO: Add on:click handler for modulo -->
        <button class="btn btn-info btn-block" on:click={modulo}>
          % Modulo
        </button>
      </div>
    </div>

    <!-- ✅ WORKING: Clear button (demonstrates state reset) -->
    <button class="btn btn-danger btn-block" on:click={clear}>
      🔄 Clear All
    </button>

    <!-- ✅ WORKING: Result Display (demonstrates reactive UI updates) -->
    <div class="result-display">
      {#if operation}
        <div>
          <div class="operation-label">{operation} Result:</div>
          <div>{result}</div>
        </div>
      {:else}
        <p style="color: #94a3b8; font-size: 1.2rem; margin: 0;">
          Enter numbers and click an operation
        </p>
      {/if}
    </div>

    <!-- Calculation History -->
    {#if historyList.length > 0}
      <div class="history-section">
        <h3>Calculation History</h3>
        <ul>
          {#each historyList as item}
            <li>{item}</li>
          {/each}
        </ul>
        <button class="btn btn-secondary btn-block" on:click={clearHistory}>
          🗑️ Clear History
        </button>
      </div>
    {/if}
  </div>
</div>

<style>
  @import url("https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css");

  /* ✅ WORKING: Gradient background */
  :global(body) {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    padding: 2rem 0;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
      Ubuntu, Cantarell, sans-serif;
  }

  /* ✅ WORKING: Calculator container styling */
  .calculator-container {
    max-width: 600px;
    margin: 3rem auto;
    padding: 2.5rem;
    background: white;
    border-radius: 20px;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  }

  /* ✅ WORKING: Result display styling */
  .result-display {
    font-size: 2.5rem;
    font-weight: bold;
    color: #667eea;
    margin-top: 2rem;
    padding: 1.5rem;
    background: linear-gradient(to right, #f7f9fc, #eef2f7);
    border-radius: 12px;
    text-align: center;
    min-height: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 2px solid #e0e7ff;
    transition: all 0.3s ease;
  }

  .result-display:has(.operation-label) {
    flex-direction: column;
    gap: 0.5rem;
  }

  .operation-label {
    font-size: 1rem;
    color: #64748b;
    font-weight: normal;
  }

  /* ✅ WORKING: Button enhancements */
  .btn {
    font-size: 1.1rem;
    padding: 0.8rem;
    font-weight: 600;
    border: none;
    transition: all 0.2s ease;
  }

  .btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  }

  .btn:active {
    transform: translateY(0);
  }

  /* ✅ WORKING: Input styling */
  input[type="number"] {
    font-size: 1.2rem;
    padding: 0.8rem;
    border: 2px solid #e0e7ff;
    border-radius: 8px;
    transition: border-color 0.2s;
  }

  input[type="number"]:focus {
    border-color: #667eea;
    outline: none;
    box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
  }

  /* ✅ WORKING: Clear button styling */
  .btn-danger {
    background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
    margin-top: 1rem;
  }

  h1 {
    color: #1e293b;
    font-weight: 700;
    margin-bottom: 2rem;
  }

  /* History section styling */
  .history-section {
    margin-top: 2rem;
    padding: 1.5rem;
    background: #f8fafc;
    border-radius: 12px;
    border: 1px solid #e2e8f0;
  }

  .history-section h3 {
    color: #334155;
    font-size: 1.5rem;
    margin-bottom: 1rem;
    text-align: center;
  }

  .history-section ul {
    list-style: none;
    padding: 0;
    margin: 0 0 1rem 0;
  }

  .history-section li {
    background: white;
    padding: 0.75rem;
    margin-bottom: 0.5rem;
    border-radius: 8px;
    border-left: 4px solid #667eea;
    font-family: "Courier New", monospace;
    font-size: 1rem;
    color: #475569;
  }

  /* ⚠️ TODO: Add custom animation for result changes (optional)
     HINTS:
     - Use @keyframes to create a fade-in or scale animation
     - Apply animation to .result-display when result updates
     - Consider pulse or slide-in effects */
</style>
