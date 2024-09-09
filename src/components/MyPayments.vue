<template>
  <div class="my-payments-container">
    <!-- Bank Accounts Section -->
    <div class="payment-section">
      <div class="section-title">Bank Accounts</div>
      <div v-if="bankAccounts.length > 0" class="payment-method">
        <div v-for="(account, index) in bankAccounts" :key="account.id" class="d-flex align-items-center justify-content-between py-3 border-bottom">
          <!-- Use the index to display the account number -->
          <div class="bank-logo me-3"><img :src="account.bankLogo" alt="Bank Logo" class="rounded" /></div>
          <div class="bank-details flex-grow-1 ml-3">
            <div class="bank-name fw-bold">{{ account.bankName }}</div>
            <div class="bank-number">{{ account.accountNumber }}</div>
            <div class="account-holder">{{ account.fullName }}</div>
            <small class="text-muted">Account #{{ index + 1 }}</small> <!-- Example of using the index -->
          </div>
          <div class="actions">
            <button @click="deleteAccount(account.id)" class="btn btn-dark btn-sm btn-lg w-100 mb-2">Delete</button>
          </div>
        </div>
      </div>
      <div v-else class="text-muted fst-italic">You don't have any bank accounts yet.</div>

      <div class="add-button-container text-center mt-3">
        <button @click="toggleAddBankAccount" class="btn btn-dark">
          {{ isAddingBankAccount ? 'Cancel' : '+ Add New Bank Account' }}
        </button>
      </div>

      <!-- Add New Bank Account Form -->
      <div v-if="isAddingBankAccount" class="add-new-form mt-3">
        <input v-model="newBankAccount.fullName" placeholder="Account Holder Name" class="form-control mb-2" />
        <input v-model="newBankAccount.accountNumber" placeholder="Account Number" class="form-control mb-2" />
        <input v-model="newBankAccount.bankName" placeholder="Bank Name" class="form-control mb-2" />
        <button @click="addNewBankAccount" class="btn btn-dark btn-sm me-2 mr-2">Save</button>
        <button @click="toggleAddBankAccount" class="btn btn-light btn-sm border-dark">Cancel</button>
      </div>
    </div>

    <!-- Bank Cards Section -->
    <div class="payment-section mt-4">
      <div class="section-title">Bank Cards</div>
      <div v-if="bankCards.length > 0" class="payment-method">
        <div v-for="(card, index) in bankCards" :key="card.id" class="d-flex align-items-center justify-content-between py-3 border-bottom">
          <!-- Use the index to display the card number -->
          <div class="card-logo me-3"><img :src="card.cardLogo" alt="Card Logo" class="rounded" /></div>
          <div class="card-details flex-grow-1 ml-3">
            <div class="card-type fw-bold">{{ card.cardType }}</div>
            <div class="card-number">{{ card.cardNumber }}</div>
            <small class="text-muted">Card #{{ index + 1 }}</small> <!-- Example of using the index -->
          </div>
          <div class="actions">
            <button @click="deleteCard(card.id)" class="btn btn-dark btn-sm">Delete</button>
          </div>
        </div>
      </div>
      <div v-else class="text-muted fst-italic">You don't have any bank cards yet.</div>

      <div class="add-button-container text-center mt-3">
        <button @click="toggleAddCard" class="btn btn-dark">
          {{ isAddingCard ? 'Cancel' : '+ Add New Card' }}
        </button>
      </div>

      <!-- Add New Card Form -->
      <div v-if="isAddingCard" class="add-new-form mt-3">
        <select v-model="newCard.cardType" class="form-control mb-2">
          <option disabled value="">Select Card Type</option>
          <option value="Visa">Visa</option>
          <option value="MasterCard">MasterCard</option>
        </select>
        <input v-model="newCard.cardNumber" placeholder="Card Number" class="form-control mb-2" />
        <input v-model="newCard.cardLogo" placeholder="Card Logo URL" class="form-control mb-2" />
        <button @click="addNewCard" class="btn btn-dark btn-sm me-2 mr-2">Save</button>
        <button @click="toggleAddCard" class="btn btn-light btn-sm border-dark">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MyPayments",
  data() {
    return {
      bankAccounts: [
        {
          id: 1,
          bankLogo: 'https://via.placeholder.com/50',
          bankName: 'MALAYAN BANKING BHD (MAYBANK)',
          accountNumber: '*1025',
          fullName: 'Rhea Thadhani',
        },
        {
          id: 2,
          bankLogo: 'https://via.placeholder.com/50',
          bankName: 'CIMB BANK',
          accountNumber: '*8743',
          fullName: 'John Doe',
        }
      ],
      bankCards: [
        {
          id: 1,
          cardLogo: 'https://via.placeholder.com/50',
          cardType: 'Visa',
          cardNumber: '**** **** **** 3445',
        },
        {
          id: 2,
          cardLogo: 'https://via.placeholder.com/50',
          cardType: 'MasterCard',
          cardNumber: '**** **** **** 9876',
        }
      ],
      isAddingBankAccount: false,
      isAddingCard: false,
      newBankAccount: {
        fullName: '',
        accountNumber: '',
        bankName: '',
      },
      newCard: {
        cardType: '',
        cardNumber: '',
        cardLogo: '',
      },
    };
  },
  methods: {
    toggleAddBankAccount() {
      this.isAddingBankAccount = !this.isAddingBankAccount;
      this.resetNewBankAccount();
    },
    toggleAddCard() {
      this.isAddingCard = !this.isAddingCard;
      this.resetNewCard();
    },
    addNewBankAccount() {
      const newAccount = {
        id: Date.now(),
        ...this.newBankAccount,
        accountNumber: `*${this.newBankAccount.accountNumber.slice(-4)}`,
      };
      this.bankAccounts.push(newAccount);
      this.toggleAddBankAccount();
    },
    addNewCard() {
      const newCard = {
        id: Date.now(),
        ...this.newCard,
        cardNumber: `**** **** **** ${this.newCard.cardNumber.slice(-4)}`,
      };
      this.bankCards.push(newCard);
      this.toggleAddCard();
    },
    deleteAccount(accountId) {
      this.bankAccounts = this.bankAccounts.filter(account => account.id !== accountId);
    },
    deleteCard(cardId) {
      this.bankCards = this.bankCards.filter(card => card.id !== cardId);
    },
    resetNewBankAccount() {
      this.newBankAccount = {
        fullName: '',
        accountNumber: '',
        bankName: '',
      };
    },
    resetNewCard() {
      this.newCard = {
        cardType: '',
        cardNumber: '',
        cardLogo: '',
      };
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
  text-align: center;
}

.payment-section {
  margin-bottom: 30px;
  padding: 15px;
  background-color: #ffffff;
}

.section-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 15px;
}

.add-new-form {
  margin-top: 20px;
}
</style>
