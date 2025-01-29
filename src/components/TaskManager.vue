<template>
  <div class="min-h-screen bg-gray-100 flex flex-col items-center p-6">
    <div
      class="w-full max-w-2xl bg-white p-4 rounded-lg shadow-lg flex justify-between items-center"
    >
      <h1 class="text-xl font-bold">Task Manager</h1>
      <div class="flex gap-4">
        <button @click="changeProfile" class="text-purple-600 hover:underline">
          Change Profile
        </button>
        <button
          @click="logout"
          class="bg-purple-600 text-white px-4 py-2 rounded-lg"
        >
          Log Out
        </button>
      </div>
    </div>

    <div class="space-y-4">
      <div class="mt-10 w-full max-w-lg flex flex-col gap-4">
        <div class="bg-white p-4 rounded-lg shadow">
          <form @submit.prevent="addTask">
            <div class="border-2 border-gray-300 rounded-lg p-2 w-full mb-4">
              <input
                v-model="newTask.title"
                placeholder="Task Title"
                required
              />
            </div>
            <div class="border-2 border-gray-300 rounded-lg p-2 w-full mb-4">
              <input
                v-model="newTask.description"
                placeholder="Task Description"
                required
              />
            </div>
            <button
              type="submit"
              class="w-full bg-purple-600 text-white py-2 rounded-lg hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-purple-500"
            >
              Add Task
            </button>
          </form>
          <ul>
            <li v-for="(task, index) in tasks" :key="index">
              <span :class="{ done: task.done }"
                >{{ task.title }} - {{ task.description }}</span
              >
              <button @click="toggleDone(index)">
                {{ task.done ? "Undo" : "Done" }}
              </button>
              <button @click="deleteTask(index)">Delete</button>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      newTask: { title: "", description: "", done: false },
    };
  },
  mounted() {
    const username = localStorage.getItem("loggedInUser");
    this.tasks = JSON.parse(localStorage.getItem(username)) || [];
  },
  methods: {
    addTask() {
      this.tasks.push({ ...this.newTask });
      this.newTask = { title: "", description: "", done: false };
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
      const username = localStorage.getItem("loggedInUser");
      localStorage.setItem(username, JSON.stringify(this.tasks));
    },
    logout() {
      localStorage.removeItem("loggedInUser");
      this.$router.push("/login");
    },
  },
};
</script>
