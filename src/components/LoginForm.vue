<template>
  <div>
    <h2>Login Page</h2>
    <form @submit.prevent="login">
      <div>
        <label for="username">Username:</label>
        <input v-model="username" type="text" required />
      </div>
      <div>
          <label for="email">Email:</label>
          <input v-model="email" type="" required />
        </div>
      <div>
        <label for="password">Password:</label>
        <input v-model="password" type="password" required />
      </div>
      <button type="submit">Login</button>
      <p v-if="error">{{ error }}</p>
    </form>
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
      const users = JSON.parse(localStorage.getItem("users")) || {};
      if (users[this.username] && users[this.username] === this.password) {
        localStorage.setItem("loggedInUser", this.username);
        this.$router.push("/tasks");
      } else {
        this.error = "Invalid username or password.";
      }
    },
  },
};
</script>
