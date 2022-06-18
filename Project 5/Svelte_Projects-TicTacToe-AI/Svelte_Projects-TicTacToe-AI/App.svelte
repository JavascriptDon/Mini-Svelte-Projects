<script>
  import Square from "./Square.svelte";
  import { winCombinations } from "./constants.js";

  let array = [];
  let usersTurn = true;
  let message = "";

  const checkWinner = winr => {
    if (array.length >= 5) {
      let arr2 = array.filter((val, i) => i % 2 == winr);
      winCombinations.forEach(array => {
        if (array.every(element => arr2.includes(element))) {
          if (winr == 0) {
            message = "You won!";
            const game = document.querySelector(".game-container");
            array.forEach((array, index) => {
              game.childNodes[array].style.backgroundColor = "#14C38E";
            });
          } else {
            message = "PC won!";
            const game = document.querySelector(".game-container");
            array.forEach((array, index) => {
              game.childNodes[array].style.backgroundColor = "#EB5353";
            });
          }
        }
      });
    }
  };
  const userMove = e => {
    if (e.target.firstChild.innerHTML == "" && !message && usersTurn) {
      e.target.firstChild.innerHTML = "X";
      array.push(parseInt(e.target.getAttribute("number")));
      checkWinner(0);
      usersTurn = false;
      if (array.length != 9) {
        pcMove();
      }
      if (array.length == 9 && !message) {
        message = "A tie!";
      }
    }
  };
  const pcMove = () => {
    if (!message) {
      setTimeout(() => {
        let num;
        do {
          num = Math.floor(Math.random() * 9);
        } while (array.includes(num));
        let arr2 = array.filter((val, i) => i % 2 == 0);
        let arr3 = array.filter((val, i) => i % 2 == 1);
        winCombinations.forEach(array => {
          if (
            array.some(element => arr2.includes(element)) &&
            !array.some(element => arr3.includes(element))
          ) {
            const findCommon = (a, b) => a.filter(n => b.indexOf(n) !== -1);
            const common = findCommon(array, arr2);
            const findMissing = (a, b) => a.filter(n => b.indexOf(n) == -1);
            const missing = findMissing(array, common);
            if (common.length == 2) {
              if (!array.includes(missing)) {
                num = missing[0];
              }
            } else if (common.length == 1) {
            }
          }
        });
        const game = document.querySelector(".game-container");
        game.childNodes[num].firstChild.innerHTML = "Y";
        array.push(num);
        usersTurn = true;
        checkWinner(1);
      }, 500);
    }
  };

  const reset = () => {
    message = "";
    array = [];
    usersTurn = true;
    const game = document.querySelector(".game-container").childNodes;
    game.forEach(el => {
      el.style.backgroundColor = "";
      el.firstChild.innerHTML = "";
    });
  };
</script>

<style>
  main {
    font-family: "Akshar", sans-serif;
    text-align: center;
    padding: 1em;
    max-width: 100%;
    margin: 0 auto;
  }
  .game-container {
    max-width: 500px;
    min-height: 500px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    gap: 10px;
  }
  .title {
    color: black;
    margin-top: 20px;
    margin-bottom: 30px;
    font-size: 46px;
  }
  .reset {
    color: black;
    margin-top: 20px;
    font-size: 26px;
    cursor: pointer;
    background-color: white;
    border: 2px solid black;
    border-radius: 15px;
    padding: 0.25em 0.5em;
    cursor: pointer;
  }

  .reset:hover {
    background-color: black;
    color: white;
    border-color: white;
  }
</style>

<main>
  <h1 class="title">{!message ? 'Tic Tac Toe' : message}</h1>
  <div class="game-container">
    {#each Array(9) as field, index}
      <Square onClick={userMove} number={index} />
    {/each}
  </div>
  <button class="reset" on:click={reset}>{!message ? 'Reset' : 'Play Again'}</button>
</main>

