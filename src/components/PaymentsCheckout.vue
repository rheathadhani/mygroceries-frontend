<template>
    <div class="payments-checkout-container card shadow-sm mx-auto">
      <!-- Card Header for Payment Details -->
      <div class="card-header bg-dark text-white text-center mb-2 py-2">
        Payment Details
      </div>
  
      <!-- Bank Cards Section -->
      <div class="card-body">
        <div class="payment-method-section">
          <p class="ml-1">Card Details</p>
          <ul class="list-group">
            <li v-for="card in bankCards" :key="card.id" class="list-group-item d-flex justify-content-between align-items-center">
              <div class="payment-item-details w-100">
                <div class="fw-bold">{{ card.cardType }}</div>
                <div>{{ card.cardNumber }}</div>
              </div>
              <div class="radio-button">
                <input type="radio" :id="'card' + card.id" name="paymentMethod" :value="card.id" />
              </div>
            </li>
          </ul>
          <div v-if="bankCards.length === 0" class="text-muted fst-italic">No card details available.</div>
        </div>
  
        <!-- Bank Accounts Section -->
        <div class="payment-method-section mt-2">
          <p class="ml-1">Bank Account</p>
          <ul class="list-group">
            <li v-for="account in bankAccounts" :key="account.id" class="list-group-item d-flex justify-content-between align-items-center">
              <div class="payment-item-details w-100">
                <div class="fw-bold">{{ account.bankName }}</div>
                <div>{{ account.accountNumber }}</div>
              </div>
              <div class="radio-button">
                <input type="radio" :id="'account' + account.id" name="paymentMethod" :value="account.id" />
              </div>
            </li>
          </ul>
          <div v-if="bankAccounts.length === 0" class="text-muted fst-italic">No bank account details available.</div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import MyPayments from './MyPayments.vue'; // Import the MyPayments data
  
  export default {
    name: 'PaymentsCheckout',
    data() {
      return {
        bankAccounts: MyPayments.data().bankAccounts, // Use data from MyPayments
        bankCards: MyPayments.data().bankCards // Use data from MyPayments
      };
    }
  };
  </script>
  
  <style scoped>
  .payments-checkout-container {
    max-width: 500px; /* Ensure the width is consistent with other components */
    margin: 0 auto; /* Center align within the parent */
    border-radius: 8px; /* Consistent border radius */
    background-color: #ffffff;
  }
  
  .card-header {
    font-size: 16px; /* Font size matching the OrderList component */
  }
  
  .payment-method-section {
    margin-bottom: 10px; /* Consistent margin for sections */
  }
  
  .list-group-item {
    padding: 8px; /* Consistent padding */
    border-bottom: 1px solid #e9ecef; /* Consistent border styling */
  }
  
  .radio-button input {
    width: 16px; /* Consistent radio button size */
    height: 16px; /* Consistent radio button size */
  }
  
  p {
    margin-bottom: 4px; /* Reduced margin */
  }
  </style>
  