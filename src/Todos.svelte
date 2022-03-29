<script>
  import FilterButton from "./components/FilterButton.svelte";
  import Todo from "./components/Todo.svelte";
  import MoreActions from "./components/MoreActions.svelte";

  export let todos = [];

  let newTodoName = "";
  let newTodoId;

  $: totalTodos = todos.length;
  $: completedTodos = todos.filter((todo) => todo.completed).length;
  $: {
    if (totalTodos === 0) {
      newTodoId = 1;
    } else {
      newTodoId = Math.max(...todos.map((t) => t.id)) + 1;
    }
  }

  function removeTodo(todo) {
    todos = todos.filter((t) => t.id !== todo.id);
  }

  const addTodo = () => {
    todos = [...todos, {id: newTodoId, name: newTodoName, completed: false}];
    newTodoName = "";
  };

  let filter = "all";
  const filterTodos = (filter, todos) =>
    filter === "active"
      ? todos.filter((t) => !t.completed)
      : filter === "completed"
      ? todos.filter((t) => t.completed)
      : todos;

  function updateTodo(todo) {
    const i = todos.findIndex((t) => t.id === todo.id);
    todos[i] = {...todos[i], ...todo};
  }

  const checkAllTodos = (completed) => {
    // Option #1
    // todos.forEach((t) => (t.completed = completed));
    // todos = todos;

    // Option #2
    // todos = todos.map((t) => {
    //   return {...t, completed: completed};
    // });

    // Option #3
    todos.forEach((t, i) => (todos[i].completed = completed));
  };

  const removeCompletedTodos = () =>
    (todos = todos.filter((t) => !t.completed));
</script>

<!-- Todos.svelte -->
<div class="todoapp stack-large">
  <!-- NewTodo -->
  <form on:submit|preventDefault={addTodo}>
    <h2 class="label-wrapper">
      <label for="todo-0" class="label__lg"> What needs to be done? </label>
    </h2>
    <input
      bind:value={newTodoName}
      type="text"
      id="todo-0"
      autocomplete="off"
      class="input input__lg"
    />
    <button type="submit" disabled="" class="btn btn__primary btn__lg">
      Add
    </button>
  </form>

  <!-- Filter -->
  <FilterButton bind:filter />

  <!-- TodosStatus -->
  <h2 id="list-heading">
    {completedTodos} out of {totalTodos} items completed
  </h2>

  <!-- Todos -->
  <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
    {#each filterTodos(filter, todos) as todo (todo.id)}
      <li class="todo">
        <Todo
          {todo}
          on:update={(e) => updateTodo(e.detail)}
          on:remove={(e) => removeTodo(e.detail)}
        />
      </li>
    {:else}
      <li>Nothing to do here!</li>
    {/each}
  </ul>

  <hr />

  <!-- MoreActions -->
  <MoreActions
    on:checkAll={(e) => checkAllTodos(e.detail)}
    on:removeCompleted={removeCompletedTodos}
  />
  <!-- <div class="btn-group">
    <button type="button" class="btn btn__primary">Check all</button>
    <button type="button" class="btn btn__primary">Remove completed</button>
  </div> -->
</div>
