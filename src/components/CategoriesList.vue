<template>
  <div class="categories-list bg-dark">
    <h2 class="text-white mb-4"> Browse <br> Categories </h2>
    <div class="list-group ">
      <CategoryItem 
        v-for="category in categories" 
        :key="category.id" 
        :category="category" 
        @click="handleCategorySelect(category.id)"
        class="list-group-item list-group-item-action mb-2"
      />
    </div>
  </div>
</template>

<script>
import CategoryItem from './CategoryItem.vue';

export default {
  name: "CategoriesList",
  components: {
    CategoryItem
  },
  data() {
    return {
      categories: [
        { id: 1, name: 'Electronics', icon: 'fas fa-tv' },
        { id: 2, name: 'Clothing', icon: 'fas fa-tshirt' },
        { id: 3, name: 'Home & Garden', icon: 'fas fa-home' },
        { id: 4, name: 'Fruits', icon: 'fas fa-apple-alt' },
        { id: 5, name: 'Fresh Meat', icon: 'fas fa-drumstick-bite' }
      ]
    };
  },
  methods: {
    handleCategorySelect(id) {
      // Emit the selected category ID to the parent component
      this.$emit('categorySelected', id);
    }
  }
};
</script>

<style scoped>
.categories-list {
  display: flex;
  flex-direction: column;
  padding: 20px;
  width: 90%; /* Adjusts to fit container width */
  
  height: auto;
  border-radius: 8px; /* Rounds the corners for a modern look */
}

h2 {
  color: #333;
  font-size: 1.8rem; /* Slightly larger and bolder for emphasis */
  margin-left: 10px;
  margin-top: 0;
  font-family: 'Roboto', sans-serif; /* Adds a modern font style */
}

.list-group-item {
  display: flex;
  align-items: center;
  padding: 15px 20px;
  border-radius: 8px;
  background-color: #fff;
  color: #333;
  font-size: 1rem;
  font-weight: 500;
  transition: all 0.3s ease;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.list-group-item:hover {
  background-color: #edf2f7;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
}

.list-group-item:last-child {
  margin-bottom: 0;
}

.list-group-item i {
  margin-right: 15px;
  font-size: 1.2rem;
}
</style>
