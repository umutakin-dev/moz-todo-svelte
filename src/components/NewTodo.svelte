<script>
  import {createEventDispatcher} from "svelte";
  const dispatch = createEventDispatcher();

  let name = "";

  const addTodo = () => {
    dispatch("addTodo", name);
    name = "";
  };

  const onCancel = () => (name = "");
</script>

<form
  on:submit|preventDefault={addTodo}
  on:keydown={(e) => e.key === "Escape" && onCancel()}
>
  <h2 class="label-wrapper">
    <label for="todo-0" class="label__lg">What needs to be done?</label>
  </h2>
  <input
    bind:value={name}
    type="text"
    id="todo-0"
    autoComplete="off"
    class="input input__lg"
  />
  <button type="submit" disabled={!name} class="btn btn__primary btn__lg"
    >Add</button
  >
</form>
