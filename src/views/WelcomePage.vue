<template>
  <div id="page-wrap-container" class="vh-100 vw-100 d-flex m-0 overflow-hidden">
    <!-- Conditional Rendering Based on showSettings -->
    <div v-if="showSettings" class="container mt-5">
      <!-- Toggle Button to Go Back to Welcome -->
      <button class="btn btn-dark mb-3" @click="toggleSettings">
        Back To Home
      </button>

      <!-- Accessibility Settings Cards -->
      <div class="row">
        <!-- Column for Color Mode -->
        <div class="col-md-4">
          <div class="card mb-3">
            <div class="card-header bg-dark text-white text-center">
              Color Mode
            </div>
            <div class="card-body">
              <p class="text-dark"><strong>Choose your preferred color mode</strong></p>
              <p class="text-dark">Light Mode (Default)</p>
              <!-- Example for Light Mode -->
              <div class="mb-3" style="background-color: #f8f9fa; color: #343a40; border: 1px solid #ddd;">
                <p class="text-dark mb-2 mt-2 sample">Sample Text</p>
              </div>
              <button class="btn btn-secondary border-dark mb-3 w-100">Switch to light mode</button>
              <p class="text-dark">Dark Mode</p>
              <!-- Example for Dark Mode -->
              <div style="background-color: #343a40; color: #ffffff; border: 1px solid #ddd;">
                <p class="text-light mb-2 mt-2 sample">Sample Text</p>
              </div>
              <button class="btn btn-secondary border-dark mt-3 w-100">Switch to Dark Mode</button>
            </div>
          </div>
        </div>

        <!-- Column for Font Settings -->
        <div class="col-md-4">
          <div class="card mb-3">
            <div class="card-header bg-dark text-white text-center">
              Font Settings
            </div>
            <div class="card-body">
              <p><strong>Adjust your screen font size</strong></p>

              <!-- Small Font Size Example -->
              <div class="mb-3">
                <h3 class="font-example">Sample Text</h3>
                <button class="btn btn-secondary border-dark w-100">Small</button>
              </div>

              <!-- Medium Font Size Example -->
              <div class="mb-3">
                <h2 class="font-example">Sample Text</h2>
                <button class="btn btn-secondary border-dark w-100">Medium</button>
              </div>

              <!-- Large Font Size Example -->
              <div class="mb-3">
                <h1 class="font-example">Sample Text</h1>
                <button class="btn btn-secondary border-dark w-100">Large</button>
              </div>
            </div>
          </div>
        </div>

        <!-- Column for Speech Settings -->
        <div class="col-md-4">
          <div class="card mb-3">
            <div class="card-header bg-dark text-white text-center">
              Speech Settings
            </div>
            <div class="card-body">
              <p><strong>Enable or disable speech feedback</strong></p>
              <button class="btn btn-light border-dark w-100 mb-3">Enable Speech</button>
              <button class="btn btn-secondary border-dark w-100 mb-4">Disable Speech</button>
              <p><strong>Choose your speech type</strong></p>
              <select id="voiceSelect" class="form-control mb-3"></select>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Default Welcome Page -->
    <div v-else class="d-flex m-0 overflow-hidden w-100">
      <div class="half-page d-flex flex-column justify-content-center align-items-center p-3 bg-dark text-white text-center col-md-5">
        <h1>Welcome to MyGroceries</h1>
        <p id="intro-text">Grocery shopping made easier and accessible</p>
        <button class="btn btn-outline-light" @click="toggleSettings">Accessibility Settings</button>
      </div>
      <div class="half-page d-flex flex-column justify-content-center align-items-center p-3 bg-white text-dark text-center col-md-7">
        <component 
          :is="currentComponent" 
          @showForgotPassword="showForgotPassword"
          @showRegistrationForm="showRegistrationForm"
          @showLoginForm="showLoginForm"
        />
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
      currentComponent: 'LoginForm',
      showSettings: false, // State to toggle accessibility settings
    };
  },
  methods: {
    showForgotPassword() {
      this.currentComponent = 'ForgotPassword';
    },
    showRegistrationForm() {
      this.currentComponent = 'RegistrationForm';
    },
    showLoginForm() {
      this.currentComponent = 'LoginForm';
    },
    toggleSettings() {
      this.showSettings = !this.showSettings; // Toggle the state

      // Call populateVoiceList only when settings are shown
      if (this.showSettings) {
        // Wait for the voices to load
        if (typeof speechSynthesis !== 'undefined') {
          speechSynthesis.onvoiceschanged = this.populateVoiceList;
        }
        this.populateVoiceList();
      }
    },
    populateVoiceList() {
      if (typeof speechSynthesis === 'undefined') {
        return;
      }

      var voices = speechSynthesis.getVoices();
      var voiceSelect = document.getElementById('voiceSelect');

      if (voiceSelect) { // Ensure the element exists before accessing it
        voiceSelect.innerHTML = ''; // Clear the existing options

        voices.forEach((voice) => {
          var option = document.createElement('option');
          option.textContent = `${voice.name} (${voice.lang})`;

          if (voice.default) {
            option.textContent += ' — DEFAULT';
          }

          option.setAttribute('data-lang', voice.lang);
          option.setAttribute('data-name', voice.name);
          voiceSelect.appendChild(option);
        });
      }
    }
  }
};
</script>

<style scoped>
/* Additional styles to complement Bootstrap */
.half-page {
  box-sizing: border-box; /* Keeps the padding inside the element dimensions */
}

h1, h2, p {
  margin-bottom: 20px;
}

#intro-text {
  font-size: 1.25rem; /* Bootstrap doesn't use vh for font-sizing by default */
}

.title {
  color: #333;
  font-weight: bold;
  margin-bottom: 20px;
}

.card {
  height: 100%;
}

.font-example {
  text-align: center;
}

.sample {
  text-align: center;
}

.card-header:hover {
  background-color: #343a40;
}

@media (max-width: 600px) { /* Adjust this breakpoint as needed */
  #text-wrap {
    display: none; /* Hide the welcome text on small screens */
  }

  #page-wrap-container {
    justify-content: center; /* Center the login form */
  }

  .half-page {
    width: 100%; /* Make the login form take full width */
  }
}

@media (max-width: 600px) {
  #page-wrap-container {
    flex-direction: column; /* Stack the items vertically */
  }
}
</style>
