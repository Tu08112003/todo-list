<script>
  export default {
    // Không lưu giá trị lại load là mất
  // data() {
  //   return {
  //     newTask: '',
  //     tasks: []
  //   };
  // },
  // methods: {
  //   addTask() {
  //     if (this.newTask.trim()) {
  //       this.tasks.push({ text: this.newTask, completed: false });
  //       this.newTask = '';
  //     }
  //   },
  //   deleteTask(index) {
  //     this.tasks.splice(index, 1);
  //   }
  // }
  //
  data() {
    return {
      newTask: '',
      tasks: []
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({ text: this.newTask, completed: false });
        this.newTask = '';
        this.saveTasks(); // Lưu lại danh sách sau khi thêm
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      this.saveTasks(); // Lưu lại danh sách sau khi xóa
    },
    saveTasks() {
      // Lưu danh sách công việc vào localStorage
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    loadTasks() {
      // Tải danh sách công việc từ localStorage
      const savedTasks = localStorage.getItem('tasks');
      if (savedTasks) {
        this.tasks = JSON.parse(savedTasks);
      }
    }
  },
  created() {
    // Tải danh sách công việc khi ứng dụng được khởi tạo
    this.loadTasks();
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
            placeholder="Add new..."
            v-on:keyup.enter="addTask"

          />
          <button v-on:click="addTask">Add</button>
        </div>
      </div>
      <div class="todo-list">
        <ul>
          <li v-for="(task, index) in tasks" :key="index">
          <input type="checkbox" 
             v-model="task.completed" 
          />
          <span :class="{ completed: task.completed }">{{ task.text }}</span>
          <button v-on:click="deleteTask(index)">Delete</button>
        </li>
        </ul>
      </div>
  </div>
</template>

<style scoped>
  .container{
    display: grid;
    grid-template-columns: 1fr, 1fr;
    gap: 2rem;
    width: 90%;
  }
  .header{
    padding: 2rem;
    align-items: center;
    text-align: center;
    color: #04AA6D;
  }
  .input-todo{
    display: grid;
    grid-template-columns: 4fr 0.5fr; 
    align-items: center;
    margin-top: 2rem;
  }
  .input-todo input{
    height: 2rem;
    border-radius: 5px;
    margin-right: 1rem;
    padding: 1rem 1rem;
  }
   .input-todo input:focus{
    outline: none;
    border: 2px solid #04AA6D 
  }
  button{
    width: 4rem;
    height: 2rem;
    border-radius: 5px;
    border: none;
    box-shadow: 3px 3px #999;
    background-color: #04AA6D;
    color: #fff;
  }
  button:hover{
    background-color: #3e8e41;
    box-shadow: 3px 3px #666;
  }
  .todo-list li{
    display: grid;
    grid-template-columns: 1fr 3fr 1fr; 
    align-items: center;
    gap: 1rem; 
    padding: 0.5rem;
    border-bottom: 1px solid #ddd; 
    list-style: none;
  }
  .todo-list li .completed {
    text-decoration: line-through;
  }

</style>
