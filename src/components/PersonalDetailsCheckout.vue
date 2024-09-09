<template>
    <div class="personal-details card shadow-sm mx-auto">
      <!-- Card Header for Personal Details -->
      <div class="card-header bg-dark text-white text-center mb-2 py-2">
        Personal Details
      </div>
      
      <!-- Form Section -->
      <div class="card-body">
        <form @submit.prevent="submitForm" method="post">
          <!-- Full Name Field -->
          <div class="form-group mb-3">
            <label for="fullName" class="form-label">Full Name</label>
            <input type="text" id="fullName" v-model="fullName" class="form-control" required />
          </div>
  
          <!-- Phone Number Field -->
          <div class="form-group mb-3">
            <label for="phoneNumber" class="form-label">Phone Number</label>
            <input type="tel" id="phoneNumber" v-model="phoneNumber" class="form-control" required />
          </div>
  
          <!-- Address Selection -->
          <div class="form-group mb-4">
            <label for="address" class="form-label">Address</label>
            <select id="address" v-model="selectedAddress" class="form-control" required>
              <option value="" disabled class="text-light">Select an address</option>
              <option v-for="(address, index) in addresses" :key="index" :value="address.fullAddress">
                {{ address.fullAddress }}
              </option>
            </select>
          </div>
  
          <!-- Submit Button -->
          <button type="submit" class="btn btn-dark w-100">Save</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  // Import MyAddresses component data
  import MyAddresses from './MyAddresses.vue';
  
  export default {
    name: 'PersonalDetailsCheckout',
    data() {
      return {
        fullName: '',
        phoneNumber: '',
        selectedAddress: '',
        addresses: MyAddresses.data().addresses, // Use data from MyAddresses
      };
    },
    methods: {
      submitForm() {
        // Handle form submission logic here
        console.log('Full Name:', this.fullName);
        console.log('Phone Number:', this.phoneNumber);
        console.log('Selected Address:', this.selectedAddress);
        // Add more submission logic as needed
      },
    },
  };
  </script>
  
  <style scoped>
  .personal-details {
    max-width: 500px; /* Match the width to other components for consistency */
    margin: 0 auto; /* Center align within the parent */
    border-radius: 8px; /* Consistent border radius */
    background-color: #ffffff;
  }
  
  .card-header {
    font-size: 16px; /* Font size matching the other components */
  }
  
  .form-label {
    color: #000;
    margin-bottom: 5px;
    text-align: left;
  }
  
  .btn {
    transition: background-color 0.3s, color 0.3s;
  }
  
  </style>
  