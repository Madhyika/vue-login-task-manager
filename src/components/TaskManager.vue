<template>
    <div>
      <h2>Task Manager</h2>
      <form @submit.prevent="addTask">
        <input v-model="newTask.title" placeholder="Task Title" required />
        <input v-model="newTask.description" placeholder="Task Description" required />
        <button type="submit">Add Task</button>
      </form>
  
      <ul>
        <li v-for="(task, index) in tasks" :key="index">
          <span :class="{ done: task.done }">{{ task.title }} - {{ task.description }}</span>
          <button @click="toggleDone(index)">{{ task.done ? 'Undo' : 'Done' }}</button>
          <button @click="deleteTask(index)">Delete</button>
        </li>
      </ul>
  
      <button @click="logout">Logout</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        tasks: [],
        newTask: { title: '', description: '', done: false },
      };
    },
    mounted() {
      const username = localStorage.getItem('loggedInUser');
      this.tasks = JSON.parse(localStorage.getItem(username)) || [];
    },
    methods: {
      addTask() {
        this.tasks.push({ ...this.newTask });
        this.newTask = { title: '', description: '', done: false };
        this.saveTasks();
      },
      toggleDone(index) {
        this.tasks[index].done = !this.tasks[index].done;
        this.saveTasks();
      },
      deleteTask(index) {
        this.tasks.splice(index, 1);
        this.saveTasks();
      },
      saveTasks() {
        const username = localStorage.getItem('loggedInUser');
        localStorage.setItem(username, JSON.stringify(this.tasks));
      },
      logout() {
        localStorage.removeItem('loggedInUser');
        this.$router.push('/login');
      },
    },
  };
  </script>
  
  <style>
  .done {
    text-decoration: line-through;
  }
  </style>
  