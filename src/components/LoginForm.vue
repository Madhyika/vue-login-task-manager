<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100">
    <div class="bg-white p-8 rounded-lg shadow-lg w-96">
      <h2 class="text-2xl font-semibold text-gray-800 text-center mb-6">
        Login Page
      </h2>
      <form @submit.prevent="login">
        <div class="mb-4">
          <label for="username" class="block text-sm text-gray-700 mb-1"
            >Username:</label
          >
          <input
            v-model="username"
            type="text"
            id="username"
            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500"
            required
          />
        </div>
        <div class="mb-4">
          <label for="email" class="block text-sm text-gray-700 mb-1"
            >Email:</label
          >
          <input
            v-model="email"
            type="email"
            id="email"
            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500"
            required
          />
        </div>
        <div class="mb-4">
          <label for="password" class="block text-sm text-gray-700 mb-1"
            >Password:</label
          >
          <input
            v-model="password"
            type="password"
            id="password"
            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500"
            required
          />
        </div>
        <div class="mb-4">
          <span class="mt-4 text-sm text-gray-600 text-center">
            Don't have an account?
            <router-link to="/register" class="text-purple-600 hover:underline"
              >Register</router-link
            >
          </span>
          <router-link to="/register"></router-link>
        </div>
        <button
          type="submit"
          class="w-full bg-purple-600 text-white py-2 rounded-lg hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-purple-500"
        >
          Login
        </button>
        <p v-if="error" class="mt-2 text-sm text-red-500 text-center">
          {{ error }}
        </p>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: "",
      email: "",
      password: "",
      error: null,
    };
  },
  methods: {
    login() {
      const users = JSON.parse(localStorage.getItem("users")) || [];

      const user = users.find(
        (u) => u.username === this.username && u.email === this.email
      );

      if (user) {
        if (user.password === this.password) {
          localStorage.setItem("loggedInUser", JSON.stringify(user));
          this.$router.push("/tasks");
        } else {
          this.error = "Invalid password.";
        }
      } else {
        this.error =
          "No user found with the given username and email. Please register.";
      }
    },
  },
};
</script>
