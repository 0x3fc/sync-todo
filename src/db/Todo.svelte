<script context="module">
  import { todoStore } from "../store/Todo.svelte";

  const db = new PouchDB("tasks-local");
  const remote = "http://localhost:5984/tasks";

  db.changes({
    since: "now",
    live: true,
    retry: true,
    continuous: true
  }).on("change", fetchTodos);

  export function fetchTodos() {
    db.allDocs({ include_docs: true, descending: true }, (_, doc) => {
      todoStore.update(_ => doc.rows);
    });
  }

  export function addTodo(content) {
    const todo = {
      _id: new Date().toISOString(),
      content,
      done: false
    };

    updateTodo(todo);
  }

  export function updateTodo(todo) {
    db.put(todo, (_, res) => {
      fetchTodos();
    });
  }

  export function deleteTodo(todo) {
    db.remove(todo, () => {});
  }

  export function sync() {
    const syncErrorHandle = () => {};

    db.replicate.to(remote, { live: true }, syncErrorHandle);
    db.replicate.from(remote, { live: true }, syncErrorHandle);
  }
</script>
