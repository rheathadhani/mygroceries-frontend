<template>
  <div class="categories-list bg-dark mb-3">
    <h3 class="text-white mb-4"> Browse <br> Categories </h3>
    <div class="list-group">
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
  border: 1px solid #ddd; /* Match the button's border color */
  border-radius: 5px; /* Similar rounding to the button */
  padding: 10px; /* Same padding as filter bar */
  background-color: #343a40; /* Keep the dark background color */
  box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075); /* Same shadow style as the filter button */
}

/* Keep all existing styles unchanged */
h3 {
  color: #fff;
  margin: 0;
  padding-left: 10px;
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


