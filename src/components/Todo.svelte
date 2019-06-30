<script>
  import {
    fetchTodos,
    addTodo,
    updateTodo,
    deleteTodo,
    sync
  } from "../db/Todo.svelte";
  import { todoStore } from "../store/Todo.svelte";
  import { onMount } from "svelte";

  let content = "";

  let todos = [];

  todoStore.subscribe(v => {
    todos = v;
  });

  onMount(() => {
    document.body.addEventListener("online", () => sync());
    fetchTodos();
    sync();
  });
</script>

<h1>Sync Todo</h1>
<label>Add Todo</label>
<input bind:value={content} />
<button
  on:click={() => {
    addTodo(content), (content = '');
  }}>
  Create
</button>

{#each todos as { doc }}
  <li>
    <input
      type="checkbox"
      bind:checked={doc.done}
      on:change={e => updateTodo(doc)} />
     {doc.content}
    <button on:click={() => deleteTodo(doc)}>Delete</button>
  </li>
{/each}
