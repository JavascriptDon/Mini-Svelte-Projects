<script>
  let userTurn = "X";
  let squares = new Array(9).fill(null);
  let winner = null;
  function restartGame() {
    userTurn = "X";
    squares = new Array(9).fill(null);
    winner = null;
  }
  function handleClick(i) {
    if (!squares[i] && !winner) {
      squares[i] = userTurn;
      switchTurn();
      winner = calculateWinner(squares);
      if (winner) userTurn = null;
    }
  }
  function switchTurn() {
    if (userTurn === "X") userTurn = "O";
    else userTurn = "X";
  }
  function calculateWinner(squares) {
    const winningCombinations = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6]
    ];
    for (let i = 0; i < winningCombinations.length; i++) {
      const [a, b, c] = winningCombinations[i];
      if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c])
        return `Winner: ${squares[a].toUpperCase()}`;
    }
    const isDraw = squares.every(square => square !== null);
    return isDraw ? "It's a draw" : null;
  }
</script>

<style>
		:root {
		  --cell-size: 100px;
		  --mark-size: calc(var(--cell-size) * 0.5);
		}
		.board {
		  display: grid;
		  justify-content: center;
		  justify-items: center;
		  align-content: center;
		  align-items: center;
		  grid-template-columns: repeat(3, auto);
		}
		.cell {
		  width: var(--cell-size);
		  height: var(--cell-size);
		  border: 1px solid black;
		  display: flex;
		  justify-content: center;
		  align-items: center;
		  position: relative;
		  cursor: pointer;
		}
		.cell:first-child,
		.cell:nth-child(2),
		.cell:nth-child(3) {
		  border-top: none;
		}
		.cell:nth-child(3n + 1) {
		  border-left: none;
		}
		.cell:nth-child(3n + 3) {
		  border-right: none;
		}
		.cell:last-child,
		.cell:nth-child(8),
		.cell:nth-child(7) {
		  border-bottom: none;
		}
		.cell.X,
		.cell.O {
		  cursor: not-allowed;
		}
		.cell.X::before,
		.cell.X::after,
		.cell.O::before {
		  background-color: black;
		}
		.board.X .cell:not(.X):not(.O):hover::before,
		.board.X .cell:not(.X):not(.O):hover::after,
		.board.O .cell:not(.X):not(.O):hover::before {
		  background-color: lightgrey;
		}
		.cell.X::before,
		.cell.X::after,
		.board.X .cell:not(.X):not(.O):hover::before,
		.board.X .cell:not(.X):not(.O):hover::after {
		  content: "";
		  position: absolute;
		  width: calc(var(--mark-size) * 0.15);
		  height: var(--mark-size);
		}
		.cell.X::before,
		.board.X .cell:not(.X):not(.O):hover::before {
		  transform: rotate(45deg);
		}
		.cell.X::after,
		.board.X .cell:not(.X):not(.O):hover::after {
		  transform: rotate(-45deg);
		}
		.cell.O::before,
		.cell.O::after,
		.board.O .cell:not(.X):not(.O):hover::before,
		.board.O .cell:not(.X):not(.O):hover::after {
		  content: "";
		  position: absolute;
		  border-radius: 50%;
		}
		.cell.O::before,
		.board.O .cell:not(.X):not(.O):hover::before {
		  width: var(--mark-size);
		  height: var(--mark-size);
		}
		.cell.O::after,
		.board.O .cell:not(.X):not(.O):hover::after {
		  width: calc(var(--mark-size) * 0.7);
		  height: calc(var(--mark-size) * 0.7);
		  background-color: white;
		}
		h1 {
		  text-align: center;
		  text-decoration: underline;
		  text-decoration-color: red;
		  margin: 0 0 1em 0;
		  padding: 0 0 0.2em 0;
		}
		.restart {
		  font-weight: bold;
		  position: absolute;
		  top: 50%;
		  padding: 20px;
		  margin: 200px 500px;
		}
</style>

<div>
{#if winner}
  <h1>{winner}</h1>
  <button class="restart" on:click={restartGame}>Restart Game</button>
{:else}
  <h1>Next Player : {userTurn}</h1>
{/if}
</div>
<div class={userTurn != null ? userTurn + ' board' : 'board'}>
  {#each squares as square, i}
    <div
      class={square != null ? square + ' cell' : 'cell'}
      on:click={() => handleClick(i)} />
  {/each}
</div>