<script>
  export default{
    data(){
      return{
        newTask: '',
        tasks: []
      };
    },
    methods:{
        addTask() {
        if (this.newTask.trim()) {
          this.tasks.push({ text: this.newTask, completed: false });
          this.newTask = '';
          this.saveTasks(); 
        }
      },
      deleteTask(index) {
        this.tasks.splice(index, 1);
        this.saveTasks(); 
      },
      saveTasks() {
        localStorage.setItem('tasks', JSON.stringify(this.tasks));
      },
      loadTasks() {
        const savedTasks = localStorage.getItem('tasks');
        if (savedTasks) {
          this.tasks = JSON.parse(savedTasks);
        }
      }
    }
  };
</script>

<template>
  <div class="container">
    <div class="header">
      <h1>My Todo-S</h1>
      <div class="input-todo">
        <input type="text"
          v-model="newTask"
          placeholder="Add...."
          v-on:keyup.enter="addTask"
        />
        <button v-on:click="addTask">Add</button>
      </div>
    </div>
    <div class="todo-list">
        <ul>
          <li v-for="(task, index) in tasks" :key="index">
            <input type="checkbox" v-model="task.completed"/>
            <label :class="{ completed: task.completed }">{{ task.text }}</label>
            <button v-on:click="deleteTask(index)">Delete</button>
          </li>
        </ul>
    </div>
  </div>
  
</template>

<style scoped>
  .container {
    display: grid;
    grid-auto-rows: 1fr 1fr;
    width: 100%;
    height: 100%;
    padding: 2rem;
    border-radius: 10px;
    box-shadow: 0 0.3rem 0.4rem rgba(0, 0, 0, 0.1);
  }

  .header {
    padding: 2rem;
    text-align: center;
    color: #2c3e50; 
  }

  .header h1 {
    font-size: 2rem;
    font-weight: bold;
  }

  .input-todo {
    margin-top: 1rem;
    align-items: center;
  }

  .input-todo input {
    width: 70%;
    padding: 0.5rem 0.5rem;
    border-radius: 5px;
    margin-right: 1rem;
    border: 2px solid #ccc;
    background-color: #ffffff; 
  }

  .input-todo input:focus {
    outline: none;
    border: 2px solid #4CAF50; 
  }

  button {
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 5px;
    font-weight: bold;
    box-shadow: 0.2rem 0.3rem #ccc;
    transition: all 0.3s ease;
  }
  .input-todo button{
    background-color: #4CAF50; 
    color: #fff;
  }
  .input-todo button:hover {
    box-shadow: 0.2rem 0.3rem #aaa;
    background-color: #45a049; 
  }

  .todo-list {
    padding: 2rem 5rem;
    border-radius: 10px;
    flex-wrap: wrap;
  }

  .todo-list ul {
    list-style: none;
    padding: 0;
    margin: 0;
    overflow-y: auto;
    max-height: 300px;
  }

  .todo-list li {
    display: grid;
    grid-template-columns: 1rem 3fr 0.1fr;
    align-items: center;
    gap: 1rem;
    border-bottom: 1px solid #ccc;
    padding: 1rem;
    background-color: #fff; 
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 0.5rem;
  }

  .todo-list li label {
    color: #333; 
  }

  .todo-list li button {
    background-color: #e74c3c; 
    color: #fff; 
  }

  .todo-list li button:hover {
    background-color: #c0392b; 
  }
   .todo-list li .completed {
    text-decoration: line-through;
  }
</style>
