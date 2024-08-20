<template>
    <div class="personal-details">
        <h2>Personal Details</h2>
        <form @submit.prevent="submitForm" method="post">
            <div class="form-group">
                <label for="fullName">Full Name</label>
                <input type="text" id="fullName" v-model="fullName" required />
            </div>

            <div class="form-group">
                <label for="phoneNumber">Phone Number</label>
                <input type="tel" id="phoneNumber" v-model="phoneNumber" required />
            </div>

            <div class="form-group">
                <label for="address">Address</label>
                <select id="address" v-model="selectedAddress" required>
                    <option v-for="(address, index) in addresses" :key="index" :value="address">
                        {{ address }}
                    </option>
                </select>
            </div>

            <button type="submit" class="submit-button">Save</button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'PersonalDetailsCheckout',
    data() {
        return {
            fullName: '',
            phoneNumber: '',
            selectedAddress: '',
            addresses: [
                '123 Main St, Kuala Lumpur',
                '456 Market Ave, Bukit Jalil',
                '789 High St, Petaling Jaya',
            ],
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
    width: 100%;
    max-width: 500px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #ffffff;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.1), 0 6px 20px 0 rgba(0, 0, 0, 0.1);
}

.personal-details h2 {
    margin-bottom: 20px;
    font-size: 1.5em;
    font-family: Arial, sans-serif;
    font-weight: bold;
    color: #333;
    text-align: center;
}

.form-group {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}

label {
    font-size: 1em;
    color: #000000;
    margin-bottom: 5px;
    text-align: left;
    /* Align labels to the left */
}

input,
select {
    width: 100%;
    padding: 10px;
    font-size: 1em;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
}

.submit-button {
    width: 100%;
    padding: 12px;
    font-size: 1em;
    font-weight: bold;
    color: #fff;
    background-color: #000;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    text-align: center;
    border: 2px solid #000;
    transition: background-color 0.3s, color 0.3s;
}

.submit-button:hover {
    background-color: #ffffff;
    color: #000;
}
</style>