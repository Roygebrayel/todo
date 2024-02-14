<!-- frontend/src/TodoApp.svelte -->
<script>
  import { createEventDispatcher } from 'svelte';

  let tasks = [];
  let newTask = '';
  const dispatcher = createEventDispatcher();

  function addTask() {
    if (newTask.trim() !== '') {
      tasks = [...tasks, { id: tasks.length + 1, text: newTask.trim(), completed: false }];
      newTask = '';
    }
  }

  function toggleTaskStatus(taskId) {
    tasks = tasks.map(task => {
      if (task.id === taskId) {
        return {
          ...task,
          completed: !task.completed
        };
      }
      return task;
    });
  }

  function deleteTask(taskId) {
    tasks = tasks.filter(task => task.id !== taskId);
  }
</script>

<div>
  <h1>Todo App</h1>

  <!-- Input field for adding new tasks -->
  <input type="text" placeholder="Add new task" bind:value={newTask} />
  <button on:click={addTask}>Add Task</button>

  <!-- List of tasks -->
  <ul>
    {#each tasks as task (task.id)}
      <li 
        class:selected={task.completed} 
        on:click={() => toggleTaskStatus(task.id)}
      >
        {task.text}
        <button on:click={(event) => {event.stopPropagation(); deleteTask(task.id);}}>Delete</button>
      </li>
    {/each}
  </ul>
</div>
