<script lang="ts">
  const numbers = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "."];
  const operations = ["/", "x", "+", "-", "="];

  let selectedOperation = "";
  let display = "0";
  let firstValue = "";
  let secondValue = "";
  let isDisplayedResults = false;

  const calculate = () => {
    const firstNum = parseFloat(firstValue);
    const secondNum = parseFloat(secondValue);
    console.log(firstNum, secondNum);
    let result: number = 0;
    switch (selectedOperation) {
      case "/":
        result = firstNum / secondNum;
        break;
      case "x":
        result = firstNum * secondNum;
        break;
      case "+":
        result = firstNum + secondNum;
        break;
      case "-":
        result = firstNum - secondNum;
        break;
    }
    return Number.isInteger(result) ? result.toString() : result.toFixed(4);
  };

  const handleOperation = (operant: string) => {
    if (!firstValue) return;
    if (operant === "=") {
      if (!secondValue) return;
      display = calculate();
      isDisplayedResults = true;
    } else {
      display = "0";
      selectedOperation = operant;
    }
  };
  const handleNumberInput = (number: string) => {
    if (isDisplayedResults) {
      handleClear();
    }
    if (number === "." && display.includes(".")) return;

    if (display === "0" && number === ".") {
      return (display = "0.");
    }
    if (!selectedOperation) {
      firstValue = display === "0" ? number : `${display}${number}`;
      display = firstValue;
    } else {
      secondValue = display === "0" ? number : `${display}${number}`;
      display = secondValue;
    }
  };
  const handleClear = () => {
    display = "0";
    selectedOperation = "";
    firstValue = "";
    secondValue = "";
    isDisplayedResults = false;
  };
</script>

<main>
  <div class="calculator">
    <div class="result">
      {display}
    </div>
    <div class="digits">
      <div class="numbers">
        <!-- loop over numbers and display button with each number -->
        <button class="btn btn-xlg" on:click={handleClear}> C </button>
        {#each numbers as number (number)}
          <button
            on:click={() => handleNumberInput(number)}
            class={`btn ${number === "0" ? "btn-lg" : null}`}
            >{number}
          </button>
        {/each}
      </div>
      <!-- loop ove operations and display button for each operation -->
      <div class="operations">
        {#each operations as operation (operation)}
          <button
            on:click={() => handleOperation(operation)}
            class={`btn ${
              selectedOperation === operation ? "btn-silver" : "btn-orange"
            }`}>{operation}</button
          >
        {/each}
      </div>
    </div>
  </div>
</main>

<style>
  :global(*) {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }
  main {
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: "Poppins", sans-serif;
  }
  .calculator {
    background-color: rgb(28, 28, 28);
    width: 300px;
    padding: 15px;
    border-radius: 7px;
    -webkit-box-shadow: 20px 20px 100px -6px rgba(0, 0, 0, 0.5);
    -moz-box-shadow: 20px 20px 100px -6px rgba(0, 0, 0, 0.5);
    box-shadow: 20px 20px 100px -6px rgba(0, 0, 0, 0.5);
  }
  .digits {
    width: 100%;
    display: flex;
    justify-content: space-between;
    gap: 10px;
    user-select: none;
  }
  .operations {
    display: flex;
    flex-direction: column;
    width: 25%;
    gap: 10px;
  }
  .numbers {
    width: 75%;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 10px;
  }
  .btn {
    width: 60px;
    height: 60px;
    border-radius: 100px;
    background-color: rgb(114, 113, 113);
    font-size: 20px;
    font-weight: bold;
    color: white;
    /* margin: 5px; */
    border: none;
    align-self: center;
    font-family: inherit;
  }
  .btn-lg {
    width: 100%;
    grid-column: 1/3;
  }
  .btn-orange {
    background-color: orange;
  }
  .btn-silver {
    background-color: silver;
  }
  .btn-xlg {
    width: 100%;
    grid-column: 1/-1;
  }
  .result {
    width: 100%;
    height: 120px;
    color: white;
    font-size: 40px;
    display: flex;
    flex-direction: row-reverse;
    margin-right: 10px;
    border: 1px solid #fff;
    border-radius: 4px;
    margin-bottom: 10px;
    overflow: hidden;
    padding-right: 5px;
  }
  .firstNumber {
    border-bottom: 1px solid orange;
  }
</style>
