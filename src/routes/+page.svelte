<script lang="ts">
  const numbers: string[] = [
    "1",
    "2",
    "3",
    "4",
    "5",
    "6",
    "7",
    "8",
    "9",
    "0",
    ".",
  ];
  const operations: string[] = ["/", "x", "-", "+", "="];
  // the states
  let selectedOperation: String = "";
  let display = "";
  let firstNumber = "";
  let secondNumber = "";
  let isDisplayingResults = false;

  const handleOperationClick = (operation: string): void => {
    if (!firstNumber) return;
    if (operation === "=") {
      if (!secondNumber) return;
      //   if those numbers are defined parse int
      const firstNum = parseInt(firstNumber);
      const secondNum = parseInt(secondNumber);

      let results = "";

      switch (selectedOperation) {
        case "/":
          results = (firstNum / secondNum).toFixed(2);
          break;
        case "x":
          results = (firstNum * secondNum).toFixed(2);
          break;
        case "+":
          results = (firstNum + secondNum).toFixed(2);
          break;
        case "-":
          results = (firstNum - secondNum).toFixed(2);
          break;
      }
      display = results;
      isDisplayingResults = true;
    }
    selectedOperation = operation;
  };
  const handleClear = () => {
    firstNumber = "";
    secondNumber = "";
    selectedOperation = "";
    display = "";
    isDisplayingResults = false;
  };
  const handleNumberClick = (number: string): void | string => {
    // so that everything clears out if you are
    if (isDisplayingResults) {
      selectedOperation = "";
      display = "";
      firstNumber = "";
      secondNumber = "";
      isDisplayingResults = false;
    }

    if (display === "" && number === "0") return;
    // what ever the current display is show it but also append this number as well
    if (number === "." && display.includes(".")) return;

    if (number === "." && display === "") {
      return (display = "0.");
    }

    // this is being done for the operations of the first and secon numbers and to ensure that ones the operation has not been selelected it is a
    if (!selectedOperation) {
      if (number === "." && display === "") {
        firstNumber = "0.";
        return (display = firstNumber);
      }

      firstNumber = `${firstNumber}${number}`;
      return (display = firstNumber);
    }
    // condition to hanle if there is a second operation
    else {
      if (number === "." && display === "") {
        secondNumber = "0.";
        return (display = secondNumber);
      }
      secondNumber = `${secondNumber}${number}`;
      return (display = secondNumber);
    }

    // display = `${display}${number}`;
  };
</script>

<main>
  <div class="calculator">
    <div class="results">
      {display}
    </div>
    <div class="digits">
      <div class="numbers">
        <button on:click={handleClear} class="btn btn-xlg"> C </button>
        <!-- sveltes version of mapping or for each  the bracket value is the unique identifier-->
        {#each numbers as number (number)}
          <button
            class={`btn ${number === "0" ? "btn-lg" : ""}`}
            on:click={() => handleNumberClick(number)}>{number}</button
          >
        {/each}
        <!-- the closing tag for the each loop -->
      </div>
      <div class="operations">
        {#each operations as operation (operation)}
          <!-- adding an event handler  -->
          <button
            class={`btn btn-orange  ${
              operation === selectedOperation ? "btn-silver" : "btn-orange"
            }`}
            on:click={handleOperationClick.bind(this, operation)}
            >{operation}</button
          >
        {/each}
      </div>
    </div>
  </div>
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .calculator {
    background-color: rgb(28, 28, 28);
    width: 240px;
    padding: 15px;
    border-radius: 7px;
    /* height: 2%; */
  }
  .digits {
    display: flex;
  }
  .operations {
    display: flex;
    flex-direction: column;
  }
  .numbers {
    display: flex;
    flex-wrap: wrap;

    width: 200px;
  }
  .btn {
    width: 50px;
    height: 50px;
    border-radius: 100px;
    background-color: rgb(114, 113, 113);
    font-size: 20px;
    font-weight: bold;
    color: white;
    margin: 5px;
    border: none;
  }
  .btn-lg {
    width: 110px;
  }
  .btn-orange {
    background-color: orange;
  }
  .btn-silver {
    background-color: silver;
  }
  .btn-xlg {
    width: 170px;
  }
  .results {
    height: 60px;
    color: white;
    font-size: 30px;
    display: flex;
    flex-direction: row-reverse;
    margin-right: 10px;
  }
</style>
