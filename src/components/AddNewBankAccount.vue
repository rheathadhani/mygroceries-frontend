<template>
    <div class="add-new-bank-account-container">
        <h2>Add Bank Account</h2>
        <form @submit.prevent="submitBankAccount">
            <div class="form-group">
                <label for="fullName">Bank Account Holder Name</label>
                <input type="text" id="fullName" v-model="fullName"
                    placeholder="Bank Acccount Holder Name" required />
            </div>
            <div class="form-group">
                <label for="accountNumber">Account No.</label>
                <input type="text" id="accountNumber" v-model="accountNumber" placeholder="Account No." required />
            </div>
            <div class="form-group">
                <label for="bankName">Bank name</label>
                <input type="text" id="bankName" v-model="bankName" placeholder="Bank name" required />
            </div>
            <div class="form-actions">
                <button type="button" @click="cancel" class="cancel-button">Cancel</button>
                <button type="submit" class="submit-button">Submit</button>
            </div>
        </form>
    </div>
</template>


<script>
export default {
    name: 'AddNewBankAccount',
    data() {
        return {
            fullName: '',
            accountNumber: '',
            bankName: '',
        };
    },
    methods: {
        submitBankAccount() {
            // Transform bank name to uppercase
            const uppercaseBankName = this.bankName.toUpperCase();

            // Mask the account number to show only one asterisk and the last 4 digits
            const maskedAccountNumber = `*${this.accountNumber.slice(-4)}`;

            // Emit the new bank account details to be added to MyPayments.vue
            this.$emit('add-bank-account', {
                id: Date.now(), // Generates a unique ID based on the current timestamp
                fullName: this.fullName,
                accountNumber: maskedAccountNumber,
                bankName: uppercaseBankName,
            });

            // Clear form fields
            this.fullName = '';
            this.accountNumber = '';
            this.bankName = '';
        },
        cancel() {
            // Emit an event to close the popup or cancel the action
            this.$emit('cancel');
        }
    }
};
</script>


<style scoped>
.add-new-bank-account-container {
    padding: 20px;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    max-width: 400px;
    margin: 0 auto;
}

h2 {
    font-size: 22px;
    margin-bottom: 20px;
    text-align: center;
}

.form-group {
    margin-bottom: 15px;
}

label {
    display: block;
    font-weight: bold;
    margin-bottom: 5px;
}

input[type="text"] {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
}

.form-actions {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
}

.submit-button,
.cancel-button {
    padding: 10px 20px;
    font-size: 16px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.submit-button {
    background-color: #000000;
    color: white;
}

.cancel-button {
    background-color: #ddd;
    color: black;
}
</style>
