<template>
    <div class="card shadow-sm">
      <div class="card-header bg-dark text-white">
        <h5 class="m-0">Customer Details</h5>
      </div>
      <div class="card-body">
        <table class="table table-striped table-bordered w-100">
          <tbody>
            <tr>
              <th>ID</th>
              <td>{{ customer.id }}</td>
            </tr>
            <tr>
              <th>Username</th>
              <td>{{ customer.username }}</td>
            </tr>
            <tr>
              <th>Name</th>
              <td>{{ customer.name }}</td>
            </tr>
            <tr>
              <th>Email</th>
              <td>{{ customer.email }}</td>
            </tr>
            <tr>
              <th>Phone Number</th>
              <td>{{ customer.phoneNumber }}</td>
            </tr>
            <tr>
              <th>Gender</th>
              <td>{{ customer.gender }}</td>
            </tr>
            <tr>
              <th>Date of Birth</th>
              <td>{{ formatDate(customer.dob) }}</td>
            </tr>
            <tr>
              <th>Shipping Addresses</th>
              <td>
                <ul>
                  <li v-for="address in customer.shippingAddresses" :key="address">{{ address }}</li>
                </ul>
              </td>
            </tr>
            <tr>
              <th>Created Time</th>
              <td>{{ formatCreatedTime(customer.createdTime) }}</td>
            </tr>
          </tbody>
        </table>
        <button class="btn btn-secondary mt-3" @click="$emit('back-to-table')">Back to Table</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "CustomersDetails",
    props: {
      customer: Object
    },
    methods: {
      formatDate(date) {
        return new Date(date).toLocaleDateString('en-US', { dateStyle: 'short' });
      },
      formatCreatedTime(dateTime) {
        return new Date(dateTime).toLocaleString('en-US', { dateStyle: 'short', timeStyle: 'short' });
      }
    }
  };
  </script>
  
  <style scoped>
  .card {
    margin-top: 20px;
  }
  </style>
  