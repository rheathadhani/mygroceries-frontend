<template>
    <div class="change-password">
        <h1>Change Password</h1>
        <form class="form-container" @submit.prevent="handleSubmit">
            <label for="old-password">Old Password:</label>
            <input type="password" id="old-password" v-model="oldPassword" required>

            <label for="new-password">New Password:</label>
            <input type="password" id="new-password" v-model="newPassword" required>

            <button type="submit">Save</button>
        </form>
    </div>
</template>

<script>
export default {
    name: "ChangePassword",
    data() {
        return {
            oldPassword: '',
            newPassword: ''
        };
    },
    methods: {
        handleSubmit() {
            // Here you would typically validate the passwords and send them to the server
            console.log('Submitting:', this.oldPassword, this.newPassword);
            // Example: axios.post('/api/change-password', { oldPassword: this.oldPassword, newPassword: this.newPassword })
            alert('Password change submitted!');
        }
    }
}
</script>

<style scoped>
h1 {
    margin-bottom: 6vh;
}

.change-password {
    font-family: Arial, sans-serif;
    max-width: 35vw;
    height: 55vh;
    margin: auto;
    margin-top: 4vw;
    padding: 20px;
    background: #fff;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.form-container {
    display: flex;
    flex-direction: column;
}

.form-container label {
    margin-bottom: 5px;
}

.form-container input {
    margin-bottom: 30px;
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.form-container button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    border: 2px solid #000;
    border-radius: 4px;
    background-color: #000;
    color: #fff;
    transition: background-color 0.3s, color 0.3s;
}

.form-container button:hover {
    background-color: #fff;
    color: #000;
}
</style>
