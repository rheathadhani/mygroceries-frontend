<template>
  <div>
    <!-- Other checkout page content -->
    <br>
    <!-- Use the OrderList component -->
    <OrderList :orders="orderItems" />
    <br>
    <!-- Use the PersonalDetailsCheckout component -->
    <PersonalDetailsCheckout />
    <br>
    <!-- Other checkout page content -->
  </div>
</template>

<script>
import OrderList from '../components/OrderList.vue';
import PersonalDetailsCheckout from '../components/PersonalDetailsCheckout.vue';

export default {  
  name: "CheckoutPage",
  components: {
    OrderList,
    PersonalDetailsCheckout,
  }
};
</script>

<style scoped>
/* Add any styles for the CheckoutPage here */
</style>
