<template>
  <div class="form-border">
    <h2>Reset Your Password</h2>
    <form @submit.prevent="resetPassword">
      <div class="input-group">
        <input type="password" placeholder="Enter your old Password" required>
      </div>
      <div class="input-group">
        <input type="password" placeholder="Enter your new password" required>
      </div>
      <button class="btn">Reset</button>
      <p> Password Changed!</p>
    </form>

    <div class="extra-links">
      <a href="#" @click.prevent="emitLoginForm">Back to Login </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ForgotPasswordForm',
  methods: {
    resetPassword() {
      // Your reset password logic here
    },
    emitLoginForm() {
      this.$emit('showLoginForm');
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
  border: 0.5px solid #797777;
  padding: 10vh 5vh;
  border-radius: 2.5vh;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
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
  border: 2px solid #000;
  width: 10vw;
  color: #000;
  padding: 10px 20px;
  cursor: pointer;
  margin-top: 30px;
  transition: background-color 0.3s, color 0.3s;
}

.btn:hover {
  background-color: #000;
  color: #fff;
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

#forgot-password{
  margin-top: 2w;
  font-size: 2vh;
  right: 2w;
}
</style>


