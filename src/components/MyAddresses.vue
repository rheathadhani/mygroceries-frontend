<template>
  <h3>My Addresses</h3>
  <div class="container mt-4">
    <div class="address-container bg-white p-4 shadow-sm rounded">
      <div class="address-list">
        <div v-for="(address, index) in addresses" :key="index" class="address-entry border-bottom pb-3 mb-3">
          <div v-if="editIndex !== index">
            <p><strong>{{ address.name }}</strong> ({{ address.phone }})</p>
            <p class="display-address">{{ address.fullAddress }}</p>
            <button class="btn btn-dark btn-sm me-2" @click="startEdit(index)">Edit</button>
            <button class="btn btn-light border-dark btn-sm" @click="deleteAddress(index)">Delete</button>
          </div>
          <div v-else>
            <input v-model="editData.name" placeholder="Name" class="form-control mb-2">
            <input v-model="editData.phone" placeholder="Phone Number" class="form-control mb-2">
            <input v-model="editData.fullAddress" placeholder="Full Address" class="form-control mb-2">
            <button class="btn btn-success btn-sm me-2" @click="saveEdit(index)">Save</button>
            <button class="btn btn-secondary btn-sm" @click="cancelEdit()">Cancel</button>
          </div>
        </div>
      </div>
      <button class="btn btn-primary w-100" @click="addAddress">+ Add New Address</button>
    </div>
  </div>
</template>


<script>
export default {
  name: "MyAddresses",
  data() {
    return {
      addresses: [
        { name: 'Rhea Thadhani', phone: '+60 11 3669 5428', fullAddress: 'Block A A-14-8, Endah Regal Condominium, Endah Regal Condomonium, Jalan 3/149e, Taman Sri Endah, W.P. Kuala Lumpur, W.P. Kuala Lumpur, 57000' },
        { name: 'Youvna Mookhram - Richa Kissoondoyal', phone: '+60 11 2808 0114', fullAddress: 'D-16-3, Endah Promenade, Block D, No. 9, Jalan 1/149e, Taman Sri Endah, Sri Petaling, 57000, Petaling W.P. Kuala Lumpur, W.P. Kuala Lumpur, 57000' }
      ],
      editIndex: -1,
      editData: {}
    }
  },
  methods: {
    addAddress() {
      const placeholder = { name: '', phone: '', fullAddress: '' };
      this.addresses.push(placeholder);
      this.startEdit(this.addresses.length - 1);
    },
    startEdit(index) {
      this.editIndex = index;
      this.editData = JSON.parse(JSON.stringify(this.addresses[index]));
    },
    saveEdit(index) {
      this.addresses.splice(index, 1, JSON.parse(JSON.stringify(this.editData)));
      this.editIndex = -1;
    },
    cancelEdit() {
      if (this.editIndex === this.addresses.length - 1 && this.addresses[this.editIndex].name === '' && this.addresses[this.editIndex].phone === '' && this.addresses[this.editIndex].fullAddress === '') {
        this.addresses.pop();
      }
      this.editIndex = -1;
    },
    deleteAddress(index) {
      if (index !== this.editIndex) {
        this.addresses.splice(index, 1);
      } else {
        alert("Finish editing the entry before deleting it.");
      }
    }
  }
}
</script>


<style scoped>
.address-container {
  width: 100%; /* Make the container take full width of the parent */
  max-width: 70vw; /* Limit the maximum width to 80% of the viewport */
  height: 70vh; /* Reduced height for the container */
  margin: 20px auto; /* Center the container horizontally */
  padding: 20px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  overflow-x: hidden; /* Prevent horizontal overflow */
}

h3 {
  font-size: 28px;
  font-weight: bold;
  text-align: center; /* Center-align the "Edit Profile" heading */
}

.address-entry {
  padding-bottom: 15px;
}

.display-address {
  line-height: 1.5;
}

button {
  margin: 5px 0;
}

.add-button {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border-radius: 4px;
  text-align: center;
  display: block;
  width: 100%;
}

.add-button:hover {
  background-color: #0056b3;
}
</style>
