<template>
  <div class="container mt-4">
    <!-- Row for Checkout Sections -->
    <div class="row">
      <!-- First Column for Personal Details and Payment Options -->
      <div class="col-md-6 mb-3">
        <PersonalDetailsCheckout />
        <br>
        <PaymentsCheckout />
      </div>
      
      <!-- Second Column for Order List and Schedule -->
      <div class="col-md-6 mb-2">
        <OrderList :orders="orderItems" />
        <br>
        <ScheduleCheckout />
      </div>
    </div>
    
    <!-- Row for Pay Now Button -->
    <div class="row">
      <div class="col-12 text-center">
        <button class="btn btn-dark btn-lg w-100">Pay Now</button>
      </div>
    </div>
  </div>
</template>

<script>
import OrderList from '../components/OrderList.vue';
import PersonalDetailsCheckout from '../components/PersonalDetailsCheckout.vue';
import PaymentsCheckout from '../components/PaymentsCheckout.vue'; 
import ScheduleCheckout from '../components/ScheduleCheckout.vue';

export default {  
  name: "CheckoutPage",
  components: {
    OrderList,
    PersonalDetailsCheckout,
    ScheduleCheckout,
    PaymentsCheckout,
  },
  data() {
    return {
      orderItems: [
        { name: 'Anchor', quantity: 1, price: 15.42 },
        { name: 'Bread', quantity: 2, price: 2.50 },
        { name: 'Cheese', quantity: 1, price: 3.00 }
      ]
    };
  }
};
</script>

<style scoped>
.container {
  max-width: 800px; /* Adjust the width as needed */
  margin: 0 auto; /* Center the container */
}

.row {
  margin-bottom: 20px; /* Add spacing between rows */
}

.btn {
  margin-top: 20px; /* Add spacing above the button */
}
</style>
