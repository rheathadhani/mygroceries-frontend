<template>
  <div class="container mt-4">
    <!-- Row for Settings Cards -->
    <div class="row">
      <!-- Column for Color Mode -->
      <div class="col-md-4 mt-4">
        <div class="card mb-3">
          <div class="card-header bg-dark text-white text-center">
            Color Mode
          </div>
          <div class="card-body">
            <p class="text-dark"><strong>Choose your preferred color mode</strong> </p>
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
      <div class="col-md-4 mt-4">
        <div class="card mb-3">
          <div class="card-header bg-dark text-white text-center">
            Font Settings
          </div>
          <div class="card-body">
            <p><strong>Adjust your screen font size</strong></p>

            <!-- Small Font Size Example -->
            <div class="mb-3">
              <h3 class="font-example">Sample Text </h3>
              <button class="btn btn-secondary border-dark w-100">Small</button>
            </div>

            <!-- Medium Font Size Example -->
            <div class="mb-3">
              <h2 class="font-example">Sample Text </h2>
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
      <div class="col-md-4 mt-4">
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
      <!-- End of cards -->
    </div>
  </div>
</template>

<script>


export default {
  name: "SettingsMainContent",
  mounted() {
    this.populateVoiceList();
    if (typeof speechSynthesis !== 'undefined' && speechSynthesis.onvoiceschanged !== undefined) {
      speechSynthesis.onvoiceschanged = this.populateVoiceList;
    }
  },
  methods: {
    populateVoiceList() {
      if (typeof speechSynthesis === 'undefined') {
        return;
      }

      var voices = speechSynthesis.getVoices();
      var voiceSelect = document.getElementById('voiceSelect');

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
    },
    // Method to show toast notification
  }
}

</script>

<style scoped>
.container {
  max-width: 100%;
  /* Full width for the container */
}

.font-example {
  text-align: center;
}

.sample {
  text-align: center;
}

.card {
  height: 100%;
}

.card-header {
  text-align: center;
}

.card-header:hover {
  background-color: #343a40;
}
</style>
