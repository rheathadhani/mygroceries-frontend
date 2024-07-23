<template>
  <div class="form-border">
    <h2>Sign In</h2>
    <form @submit.prevent="login">
      <div class="input-group">
        <input type="email" placeholder="Enter your email address" required>
      </div>
      <div class="input-group">
        <input type="password" placeholder="Enter password" required>
      </div>
      <button class="btn">LOGIN</button>
    </form>

    <div class="extra-links">
      <a href="#" @click.prevent="emitForgotPassword">Forgot password?</a>
      <a href="#" @click.prevent="emitRegistrationForm">Register</a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LoginForm',
  methods: {
    login() {
      // Your login logic here
    },
    emitForgotPassword() {
      this.$emit('showForgotPassword');
    },
    emitRegistrationForm() {
      this.$emit('showRegistrationForm');
    }
  }
};
</script>


<style scoped>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    overflow: hidden;
}

#page-wrap-container {
  display: flex;
  height: 97vh; /* Full viewport height */
  width: 98vw; /* Full viewport width */
  margin: 0;
}

.half-page {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.right {
  background-color: #fff;
  color: #000;
  text-align: center;
  width: 60%;
}

.form-border {
  border: 1px solid #000;
  padding: 10vh 5vh;
  border-radius: 2vh;
}

h1, h2, p {
  margin-bottom: 20px;
}

#intro-text {
  margin-bottom: 20px;
  font-size: 3vh;
}

.btn {
  background-color: transparent;
  border: 2px solid #fff;
  color: #fff;
  padding: 10px 20px;
  cursor: pointer;
  text-transform: uppercase;
  transition: background-color 0.3s, color 0.3s;
}

.btn:hover {
  background-color: #fff;
  color: #000;
}

.input-group {
  margin-bottom: 20px;
  width: 30vw;
}

input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.extra-links {
  margin-top: 5vh;
}

.extra-links a {
  color: #000;
  text-decoration: none;
  margin: 0 10px;
}

.extra-links a:hover {
  text-decoration: underline;
}
</style>


]