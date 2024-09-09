<template>
    <div class="schedule-checkout card shadow-sm mx-auto">
      <!-- Card Header for Delivery Schedule -->
      <div class="card-header bg-dark text-white text-center mb-2 py-2">
        Delivery Schedule
      </div>
      
      <!-- Card Body for Date and Time Selection -->
      <div class="card-body">
        <form @submit.prevent="submitSchedule">
          <!-- Date and Time Picker -->
          <div class="form-group mb-3">
            <label for="dateTime" class="form-label">Pick a date & time</label>
            <input type="datetime-local" id="dateTime" v-model="selectedDateTime" class="form-control" required />
          </div>
  
          <!-- Submit Button -->
          <button type="submit" class="btn btn-dark w-100">Save</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "ScheduleCheckout",
    data() {
      return {
        selectedDateTime: '',
      };
    },
    methods: {
      submitSchedule() {
        // Handle the schedule submission logic here
        console.log('Selected Date & Time:', this.selectedDateTime);
        // Add more submission logic as needed
      },
    },
  };
  </script>
  
  <style scoped>
  .schedule-checkout {
    max-width: 500px; /* Match the width to other components for consistency */
    margin: 0 auto; /* Center align within the parent */
    border-radius: 8px; /* Consistent border radius */
    background-color: #ffffff;
  }
  
  .card-header {
    font-size: 16px; /* Font size matching the other components */
  }
  
  .form-label {
    font-size: 1em;
    color: #000;
    margin-bottom: 5px;
    text-align: left;
  }
  
  .btn {
    font-weight: bold;
    transition: background-color 0.3s, color 0.3s;
  }
  
  .btn:hover {
    background-color: #ffffff;
    color: #000;
  }
  </style>
  