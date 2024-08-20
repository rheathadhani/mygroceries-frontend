<template>
    <div class="add-new-card-container">
        <h2>Add New Card</h2>
        <form @submit.prevent="submitCard">
            <div class="form-group">
                <label for="cardType">Card Type</label>
                <select v-model="cardType" id="cardType" required>
                    <option value="Visa">Visa</option>
                    <option value="MasterCard">MasterCard</option>
                </select>
            </div>
            <div class="form-group">
                <label for="cardNumber">Card Number</label>
                <input type="text" id="cardNumber" v-model="cardNumber" placeholder="**** **** **** ****" maxlength="19"
                    required />
            </div>
            <div class="form-group">
                <label for="expiryDate">Expiry Date (MM/YY)</label>
                <input type="text" id="expiryDate" v-model="expiryDate" placeholder="MM/YY" maxlength="5" required />
            </div>
            <div class="form-group">
                <label for="cvv">CVV</label>
                <input type="password" id="cvv" v-model="cvv" placeholder="***" maxlength="3" required />
            </div>
            <div class="form-group">
                <label for="nameOnCard">Name on Card</label>
                <input type="text" id="nameOnCard" v-model="nameOnCard" placeholder="Name on Card" required />
            </div>
            <div class="form-group">
                <label for="cardLogo">Card Logo URL</label>
                <input type="text" id="cardLogo" v-model="cardLogo" placeholder="URL to card logo image" required />
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
    name: 'AddNewCard',
    data() {
        return {
            cardType: '',
            cardNumber: '',
            expiryDate: '',
            cvv: '',
            nameOnCard: '',
            cardLogo: '',
        };
    },
    methods: {
        submitCard() {

            const maskedCardNumber = this.cardNumber.slice(0, -4).replace(/\d/g, '*')
            .replace(/(.{4})/g, '$1 ') + this.cardNumber.slice(-4).replace(/(.{4})/g, '$1 ');

            // Emit the new card details to be added to MyPayments.vue
            this.$emit('add-card', {
                id: Date.now(), // Generates a unique ID based on the current timestamp
                cardType: this.cardType,
                cardNumber: maskedCardNumber,
                expiryDate: this.expiryDate,
                cvv: this.cvv,
                nameOnCard: this.nameOnCard,
                cardLogo: this.cardLogo,
            });

            // Clear form fields
            this.cardType = '';
            this.cardNumber = '';
            this.expiryDate = '';
            this.cvv = '';
            this.nameOnCard = '';
            this.cardLogo = '';
        },
        cancel() {
            // Emit an event to close the popup or cancel the action
            this.$emit('cancel');
        }
    }
};
</script>

<style scoped>
.add-new-card-container {
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

input[type="text"],
input[type="password"],
select {
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
    background-color: #000;
    color: white;
}

.cancel-button {
    background-color: #ddd;
    color: black;
}
</style>