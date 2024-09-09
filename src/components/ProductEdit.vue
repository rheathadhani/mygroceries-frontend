<template>
  <div class="row mb-4">
    <div class="col-12">
      <div class="card shadow-sm">
        <div class="card-header category bg-dark text-white d-flex justify-content-between align-items-center">
          <h5 class="m-0">Edit Product Details</h5>
        </div>
        <div class="card-body">
          <form @submit.prevent="save">
            <div class="mb-3">
              <label for="productName" class="form-label">Name</label>
              <input type="text" id="productName" v-model="editableProduct.name" class="form-control">
            </div>
            <div class="mb-3">
              <label for="productPrice" class="form-label">Price</label>
              <input type="number" id="productPrice" v-model="editableProduct.price" class="form-control">
            </div>
            <div class="mb-3">
              <label for="productCategory" class="form-label">Category</label>
              <select id="productCategory" v-model="editableProduct.category" class="form-control">
                <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
              </select>
            </div>
            <div class="mb-3">
              <label for="productDescription" class="form-label">Description</label>
              <textarea id="productDescription" v-model="editableProduct.description" class="form-control"></textarea>
            </div>
            <div class="mb-3">
              <label for="productImage" class="form-label">Image</label>
              <input type="file" id="productImage" @change="onFileChange" class="form-control">
            </div>
            <button type="submit" class="btn btn-success me-2">Save</button>
            <button type="button" class="btn btn-secondary" @click="cancelEdit">Cancel</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductEdit",
  props: {
    product: Object,
    categories: Array,
  },
  data() {
    return {
      editableProduct: { ...this.product }, // Create a copy of the product data for editing
    };
  },
  methods: {
    save() {
      this.editableProduct.updatedDate = new Date().toISOString(); // Update 'updatedDate' to current date and time
      this.$emit('save-product', this.editableProduct); // Emit the updated product to the parent component
    },
    cancelEdit() {
      this.$emit('back-to-table'); // Emit event to go back to table view without saving changes
    },
    onFileChange(event) {
      const file = event.target.files[0];
      if (file) {
        this.editableProduct.image = URL.createObjectURL(file); // Update image preview
      }
    },
  },
};
</script>
