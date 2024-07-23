<template>
  <div id="page-wrap-container">
    <div id="text-wrap" class="half-page left">
      <h1>Welcome to MyGroceries</h1>
      <p id = "intro-text">Grocery shopping made easier and accessible</p>
      <!--Ask Supervisor-->
      <button class="btn">Accessibility</button>
      <div class="half-page right">
        <component :is="currentComponent" @showForgotPassword="showForgotPassword" @showRegistrationForm="showRegistrationForm" />
      </div>
    </div>
  </div>
</template>

<script>
import LoginForm from '../components/LoginForm.vue';
import ForgotPassword from '../components/ForgotPasswordForm.vue';
import RegistrationForm from '../components/RegistrationForm.vue';

export default {
  name: 'WelcomePage',
  components: {
    LoginForm,
    ForgotPassword,
    RegistrationForm
  },
  data() {
    return {
      currentComponent: 'LoginForm' // Default component
    };
  },
  methods: {
    showForgotPassword() {
      this.currentComponent = 'ForgotPassword';
    },
    showRegistrationForm() {
      this.currentComponent = 'RegistrationForm';
    }
  }
};
</script>

<style scoped>
*{
  box-sizing: border-box;
}
html, body {
  height: 100%;
  margin: 0;
  overflow: hidden; /* Prevent scrolling */
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
  box-sizing: border-box; /* Include padding in width/height calculations */
}

.left {
  background-color: #000;
  color: #fff;
  text-align: center;
  width: 40%; /* Occupies 50% of the width */
  height: 100%; /* Full height of the container */
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
