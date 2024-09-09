<template>
    <div class="card shadow-sm">
      <div class="card-body">
        <form @submit.prevent="addCategory">
          <div class="mb-3">
            <label for="categoryName" class="form-label">Name</label>
            <input type="text" v-model="category.name" class="form-control" id="categoryName" required>
          </div>
          <div class="mb-3">
            <label for="numberOfProducts" class="form-label">Number of Products</label>
            <input type="number" v-model="category.numProducts" class="form-control" id="numberOfProducts" required>
          </div>
          <button type="submit" class="btn btn-primary">Save</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "AddNewCategory",
    data() {
      return {
        category: {
          name: '',
          numProducts: 0,
        }
      };
    },
    methods: {
      addCategory() {
        // Emit the new category data to the parent component
        this.$emit('save-category', this.category);
      }
    }
  };
  </script>
  
  <style scoped>
  /* Add any necessary styles here */
  </style>
  