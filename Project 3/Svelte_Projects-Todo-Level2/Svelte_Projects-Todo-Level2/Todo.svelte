<script>
  import TodoItem from "./TodoItem.svelte";

  let newTodoText = "";
  let currentFilter = "all";
  let nextId = 4;
  let todoList = [
    {
      id: 1,
      text: "Write a Svelte Todo App",
      completed: true
    },
    {
      id: 2,
      text: "Hit the ↵ button to add a new item.",
      completed: false
    },
    {
      id: 3,
      text: "Hit this ❌ to delete an item.",
      completed: false
    }
  ];
  function addTodo(event) {
    if (event.key === "Enter") {
      todoList = [
        ...todoList,
        {
          id: nextId,
          completed: false,
          text: newTodoText
        }
      ];
      nextId = nextId + 1;
      newTodoText = "";
    }
  }
  $: todoListRemaining = filteredtodoList.filter(todo => !todo.completed).length;
  $: filteredtodoList =
    currentFilter === "all"
      ? todoList
      : currentFilter === "completed"
      ? todoList.filter(todo => todo.completed)
      : todoList.filter(todo => !todo.completed);
  function checkAlltodoList(event) {
    todoList.forEach(todo => (todo.completed = event.target.checked));
    todoList = todoList;
  }
  function updateFilter(newFilter) {
    currentFilter = newFilter;
  }
  function clearCompleted() {
    todoList = todoList.filter(todo => !todo.completed);
  }
  function handleDeleteTodo(event) {
    todoList = todoList.filter(todo => todo.id !== event.detail.id);
  }
  function handleToggleComplete(event) {
    const todoIndex = todoList.findIndex(todo => todo.id === event.detail.id);
    const updatedTodo = {
      ...todoList[todoIndex],
      completed: !todoList[todoIndex].completed
    };
    todoList = [
      ...todoList.slice(0, todoIndex),
      updatedTodo,
      ...todoList.slice(todoIndex + 1)
    ];
  }
</script>

<style>
  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
    text-align: center;
  }
  .container-fluid {
    max-width: 800px;
    margin: 10px auto;
  }
  .todo-input {
    width: 100%;
    padding: 10px, 20px;
    font-size: 18px;
    margin-bottom: 20px;
  }
  .inner-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 15px;
    margin-bottom: 13px;
  }
  .inner-container-input {
    margin-right: 12px;
  }
  button {
    font-size: 14px;
    background-color: white;
    appearance: none;
    border: none;
  }
  button:hover {
    background: #ff3e00;
    color: white;
  }
  button:focus {
    outline: none;
  }
  .active {
    background: #ff3e00;
    color: white;
  }
</style>

<div class="container-fluid">
    <h1>TODO</h1>
    <input type="text" class="todo-input" placeholder="New Item ..." bind:value={newTodoText} on:keydown={addTodo} >

    {#each filteredtodoList as todo}
        <div class="todo-item">
            <TodoItem {...todo} on:deleteTodo={handleDeleteTodo} on:toggleComplete={handleToggleComplete} />
        </div>
    {/each}

    <div class="inner-container">
        <div><label><input class="inner-container-input" type="checkbox" on:change={checkAlltodoList}>Check All</label></div>
        <div>{todoListRemaining} items left</div>
    </div>

    <div class="inner-container">
        <div>
            <button on:click={() => updateFilter('all')} class:active="{currentFilter === 'all'}">All</button>
            <button on:click={() => updateFilter('active')} class:active="{currentFilter === 'active'}">Active</button>
            <button on:click={() => updateFilter('completed')} class:active="{currentFilter === 'completed'}">Completed</button>
        </div>
        <dir>
            <button on:click={clearCompleted}>Clear Completed</button>
        </dir>
    </div>

</div>