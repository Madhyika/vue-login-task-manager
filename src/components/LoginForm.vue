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
        <input v-model="email" type="email" required />
      </div>
      <div>
        <label for="password">Password:</label>
        <input v-model="password" type="password" required />
      </div>
      <div>
        <span> Don't have an account? </span>
        <router-link to="/register">Register</router-link>
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
