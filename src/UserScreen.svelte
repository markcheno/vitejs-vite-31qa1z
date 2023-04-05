<script lang="ts">
  import { pocketbase, currentUser, errorMessage } from "./lib/pocketbase";
  
  import { afterUpdate } from 'svelte';

  async function handleSignOut() {
    try {
      console.log("signing out")
      pocketbase.authStore.clear();
    } catch (error) {
      errorMessage(error);
    }
  }

  let todoItems = [];
  let newTodo = '';

  function addTodo() {
    newTodo = newTodo.trim();
    if (!newTodo) return;

    const todo = {
      text: newTodo,
      checked: false,
      id: Date.now(),
    };

    todoItems = [...todoItems, todo];
    newTodo = '';
  }

  function toggleDone(id) {
    const index = todoItems.findIndex(item => item.id === Number(id));
    todoItems[index].checked = !todoItems[index].checked;
  }

  function deleteTodo(id) {
    todoItems = todoItems.filter(item => item.id !== Number(id));
  }

  afterUpdate(() => {
    document.querySelector('.js-todo-input').focus();
  });

</script>


<ul>
  {#each todoItems as todo (todo.id)}
    <li>
      <input id={todo.id} type="checkbox" />
      <label for={todo.id} on:click={() => toggleDone(todo.id)}></label>
      <span>{todo.text}</span>
      <button  on:click={() => deleteTodo(todo.id)}>x</button>
    </li>
  {/each}
</ul>
<form on:submit|preventDefault={addTodo}>
  <input class="js-todo-input" type="text" aria-label="Enter a new todo item" placeholder="E.g. Build a web app" bind:value={newTodo}>
</form>


<section>
  <!-- <header>{$currentUser.email}</header> -->
  <button 
    on:click={handleSignOut}>
    Sign Out
  </button>
</section>
