<script>
	let newItem = "";
	let todoList = [
	  { text: "Write a Svelte Todo App", status: true },
	  { text: "Hit the ✔ button to add a new item.", status: false },
	  { text: "Hit this ❌ to delete an item.", status: false }
	];

	function addTodo() {
	  todoList = [...todoList, { text: newItem, status: false }];
	  newItem = "";
	}

	function removeTodo(index) {
	  todoList.splice(index, 1);
	  todoList = todoList;
	}
</script>


<main>
 <h1>ToDo</h1>
 <input bind:value={newItem} type="text" placeholder="New Item">
 <button on:click={addTodo}>✔</button>
 <br />
 {#each todoList as item, index}
 <input bind:checked={item.status} type="checkbox">
 <span class:checked={item.status}>{item.text}</span>
 <span on:click={() => removeTodo(index)}>❌</span>
 <br/>
 {/each} 
</main>


<style>
	* {
	  margin-top: 30px;
	}

	.checked {
	  text-decoration-color: #a683e3;
	  text-decoration: line-through;
	}

	main {
	  height: 100vh;
	  background: #fff;
	  text-align: center;
	  width: 100%;
	  font-family: sans-serif;
	}

	h1 {
	  color: #ff3e00;
	  text-transform: uppercase;
	  font-size: 4em;
	  font-weight: 100;
	}

	button {
	  background: lightgreen;
	  font-weight: bold;
	}

	@media (min-width: 640px) {
	  main {
	    max-width: none;
	  }
	}
</style>

