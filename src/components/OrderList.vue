<template>
    <div class="order-list card shadow-sm">
      <!-- Card Header for Order Details -->
      <div class="card-header bg-dark text-white text-center mb-2 py-2">
        Your Order from MyGroceries
      </div>
      
      <!-- Order Items List -->
      <div class="card-body">
        <ul class="list-group">
          <li v-for="(item, index) in orders" :key="index" class="list-group-item d-flex justify-content-between align-items-center">
            <div class="item-details d-flex justify-content-between w-100">
              <span class="item-name">{{ item.name }}</span>
              <span class="item-quantity">x{{ item.quantity }}</span>
            </div>
            <span class="badge bg-dark rounded-pill p-2 text-white ml-2">{{ formatPrice(item.price) }}</span>
          </li>
        </ul>
      </div>
  
      <!-- Order Total -->
      <div class="card-footer d-flex justify-content-between" style="background-color:#ffffff;;">
        <span>Total:</span>
        <span class="fw-bold">{{ formatPrice(totalPrice) }}</span>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'OrderList',
    props: {
      orders: {
        type: Array,
        required: true,
        default: () => [
          { name: 'Anchor', quantity: 1, price: 15.42 },
          { name: 'Bread', quantity: 2, price: 2.50 },
          { name: 'Cheese', quantity: 1, price: 3.00 }
        ],
      },
    },
    computed: {
      totalPrice() {
        return this.orders.reduce((total, item) => {
          return total + item.price * item.quantity;
        }, 0);
      },
    },
    methods: {
      formatPrice(value) {
        return `RM ${value.toFixed(2)}`;
      },
    },
  };
  </script>
  
  <style scoped>
  .order-list {
    max-width: 500px; /* Match the width to the PaymentsCheckout component */
    margin: 0 auto; /* Center align within the parent */
    border-radius: 8px;
    background-color: #ffffff;
  }
  
  .card-header {
    font-size: 16px;
  }
  
  .item-name {
    font-weight: normal;
    overflow: hidden;
    text-overflow: ellipsis; /* Add ellipsis if the text is too long */
    white-space: nowrap; /* Prevent text from wrapping to a new line */
  }
  
  .item-quantity {
    font-weight: bold;
  }
  
  .card-footer {
    border-top: 1px solid #ccc; /* Consistent styling with other sections */
  }
  </style>
  