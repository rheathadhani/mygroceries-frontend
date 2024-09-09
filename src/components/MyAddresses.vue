<template>
  <div class="container mt-4">
    <div class="address-container bg-white p-4 shadow-sm rounded border-dark shadow-sm">
      <div class="address-list">
        <div v-for="(address, index) in addresses" :key="index" class="address-entry border-bottom pb-3 mb-3">
          <div v-if="editIndex !== index">
            <p><strong>{{ address.name }}</strong> ({{ address.phone }})</p>
            <p class="display-address">{{ address.fullAddress }}</p>
            <button class="btn btn-dark btn-sm me-2 mr-2" @click="startEdit(index)">Edit</button>
            <button class="btn btn-light btn-sm border-dark" @click="deleteAddress(index)">Delete</button>
          </div>
          <div v-else>
            <input v-model="editData.name" placeholder="Name" class="form-control mb-2">
            <input v-model="editData.phone" placeholder="Phone Number" class="form-control mb-2">
            <input v-model="editData.fullAddress" placeholder="Full Address" class="form-control mb-2">
            <button class="btn btn-dark btn-sm me-2 mr-2" @click="saveEdit(index)">Save</button>
            <button class="btn btn-light btn-sm border-dark" @click="cancelEdit()">Cancel</button>
          </div>
        </div>
      </div>
      <button class="btn btn-dark w-100" @click="addAddress">+ Add New Address</button>
    </div>
  </div>
</template>


<script>
export default {
  name: "MyAddresses",
  data() {
    return {
      addresses: [
        { name: 'Rhea Thadhani', phone: '01136695428', fullAddress: 'Block A A-14-8, Endah Regal Condominium, Endah Regal Condomonium, Jalan 3/149e, Taman Sri Endah, W.P. Kuala Lumpur, W.P. Kuala Lumpur, 57000' },
        { name: 'Youvna Mookhram - Richa Kissoondoyal', phone: '01128080114', fullAddress: 'D-16-3, Endah Promenade, Block D, No. 9, Jalan 1/149e, Taman Sri Endah, Sri Petaling, 57000, Petaling W.P. Kuala Lumpur, W.P. Kuala Lumpur, 57000' }
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
  width: 100%;
  max-width: 70vw;
  height: 70vh;
  margin: 20px auto;
  padding: 20px;
  background-color: #fff;
  overflow-x: hidden;
  /* Prevent horizontal overflow */
}

h3 {
  text-align: center;
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
  color: white;
  padding: 10px 20px;
  border-radius: 4px;
  text-align: center;
  display: block;
  width: 100%;
}
</style>
