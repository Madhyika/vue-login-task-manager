<template>
    <div>
      <h2>Register</h2>
      <form @submit.prevent="register">
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
        <button type="submit">Register</button>
        <p v-if="error">{{ error }}</p>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        username: '',
        email:'',
        password: '',
        ConfirmPassword:'',
        error: null,
      };
    },
    methods: {
      register() {
        const users = JSON.parse(localStorage.getItem('users')) || {};
        if (users[this.username]) {
          this.error = 'Username already exists.';
        } else {
          users[this.username] = this.password;
          localStorage.setItem('users', JSON.stringify(users));
          this.$router.push('/login');
        }
      },
    },
  };
  </script>
  