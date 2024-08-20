<template>
  <div class="my-payments-container">
    <h3>My Payments</h3>

    <!-- Bank Accounts Section -->
    <div class="payment-section">
      <div class="section-title">Bank Accounts</div>
      <div v-if="bankAccounts.length > 0" class="payment-method">
        <div v-for="account in bankAccounts" :key="account.id" class="bank-account">
          <div class="bank-logo"><img :src="account.bankLogo" alt="Bank Logo" /></div>
          <div class="bank-details">
            <div class="bank-name">{{ account.bankName }}</div>
            <div class="bank-number">{{ account.accountNumber }}</div>
            <div class="account-holder">{{ account.fullName }}</div> <!-- Display the full name here -->
          </div>
          <div class="actions">
            <button @click="deleteAccount(account.id)">Delete</button>
          </div>
        </div>
      </div>
      <div v-else class="no-payment-methods">You don't have any bank accounts yet.</div>

      <div class="add-button-container">
        <button @click="toggleAddBankAccountPopup" class="add-button">+ Add New Bank Account</button>
      </div>
    </div>

    <!-- Bank Cards Section -->
    <div class="payment-section">
      <div class="section-title">Bank Cards</div>
      <div v-if="bankCards.length > 0" class="payment-method">
        <div v-for="card in bankCards" :key="card.id" class="bank-card">
          <div class="card-logo"><img :src="card.cardLogo" alt="Card Logo" /></div>
          <div class="card-details">
            <div class="card-type">{{ card.cardType }}</div>
            <div class="card-number">{{ card.cardNumber }}</div>
          </div>
          <div class="actions">
            <button @click="deleteCard(card.id)">Delete</button>
          </div>
        </div>
      </div>
      <div v-else class="no-payment-methods">You don't have any bank cards yet.</div>
      <div class="add-button-container">
        <button @click="toggleAddCardPopup" class="add-button">+ Add New Card</button>
      </div>
    </div>

    <!-- Add New Card Popup -->
    <div class="modal-container" v-if="showAddCardPopup">
      <div class="modal">
        <AddNewCard @add-card="addCard" @cancel="toggleAddCardPopup" />
      </div>
    </div>

    <!-- Add New Bank Account Popup -->
    <div class="modal-container" v-if="showAddBankAccountPopup">
      <div class="modal">
        <AddNewBankAccount @add-bank-account="addNewBankAccount" @cancel="toggleAddBankAccountPopup" />
      </div>
    </div>
  </div>
</template>

<script>
import AddNewCard from './AddNewCard.vue';
import AddNewBankAccount from './AddNewBankAccount.vue';

export default {
  name: "MyPayments",
  components: {
    AddNewCard,
    AddNewBankAccount
  },
  data() {
    return {
      bankAccounts: [
        {
          id: 1,
          bankLogo: 'https://via.placeholder.com/50', // Placeholder image URL
          bankName: 'MALAYAN BANKING BHD (MAYBANK)',
          accountNumber: '*1025',
          fullName: 'Rhea Thadhani',  // Display this name
        },
        {
          id: 2,
          bankLogo: 'https://via.placeholder.com/50', // Placeholder image URL
          bankName: 'CIMB BANK',
          accountNumber: '*8743',
          fullName: 'John Doe',  // Display this name
        }
      ],
      bankCards: [
        {
          id: 1,
          cardLogo: 'https://via.placeholder.com/50', // Placeholder image URL
          cardType: 'Visa',
          cardNumber: '**** **** **** 3445',
        },
        {
          id: 2,
          cardLogo: 'https://via.placeholder.com/50', // Placeholder image URL
          cardType: 'MasterCard',
          cardNumber: '**** **** **** 9876',
        }
      ],
      showAddCardPopup: false, 
      showAddBankAccountPopup: false, 
    };
  },
  methods: {
    toggleAddCardPopup() {
      this.showAddCardPopup = !this.showAddCardPopup;
    },
    toggleAddBankAccountPopup() {
      this.showAddBankAccountPopup = !this.showAddBankAccountPopup;
    },
    addCard(newCard) {
      this.bankCards.push(newCard);
      this.toggleAddCardPopup(); // Close the popup after adding the card
    },
    addNewBankAccount(newAccount) {
      this.bankAccounts.push(newAccount);
      this.toggleAddBankAccountPopup(); // Close the popup after adding the bank account
    },
    deleteAccount(accountId) {
      // Sample deletion logic
      this.bankAccounts = this.bankAccounts.filter(account => account.id !== accountId);
    },
    deleteCard(cardId) {
      // Sample deletion logic
      this.bankCards = this.bankCards.filter(card => card.id !== cardId);
    }
  }
};
</script>

<style scoped>
.my-payments-container {
  padding: 20px;
  font-family: Arial, sans-serif;
  position: relative;
}

h3 {
  font-size: 28px;
  font-weight: bold;
  margin-bottom: 20px;
}

.payment-section {
  margin-bottom: 30px;
  padding: 15px;
  background-color: #ffffff;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.1), 0 6px 20px 0 rgba(0, 0, 0, 0.1);
}

.section-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 15px;
}

.payment-method {
  margin-bottom: 20px;
}

.bank-account,
.bank-card {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid #ddd;
}

.bank-account:last-child,
.bank-card:last-child {
  border-bottom: none;
}

.bank-details,
.card-details {
  flex: 1;
  margin-left: 15px;
}

.bank-logo img,
.card-logo img {
  max-width: 50px;
  border-radius: 5px;
}

.bank-name,
.card-type {
  font-weight: bold;
  margin-bottom: 5px;
}

.actions {
  text-align: right;
}

.actions button {
  margin-left: 10px;
  padding: 5px 15px;
  width: 10vw;
  height: 5vh;
  background-color: #000000;
  color: white;
  border: 2px solid #000;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s, color 0.3s;
}

.actions button:hover {
  background-color: #fff;
  color: #000;
}

.add-button-container {
  text-align: center;
  margin-top: 10px;
}

.add-button {
  padding: 10px 20px;
  background-color: #000;
  color: white;
  border: 2px solid #000;
  border-radius: 5px;
  transition: background-color 0.3s, color 0.3s;
  cursor: pointer;
}

.add-button:hover {
  background-color: #fff;
  color: #000;
}

.no-payment-methods {
  color: #666;
  font-style: italic;
  margin-bottom: 15px;
}

.modal-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(0, 0, 0, 0.3); /* Adjusted to reduce darkness */
  transition: z-index 0.5s;
  z-index: 1000;
  overflow-y: auto;
}

.modal {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  width: 90%;
  max-width: 400px;
  z-index: 1001; /* Ensure the modal is above the background */
}
</style>
