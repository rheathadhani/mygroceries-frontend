<template>
  <div id="page-wrap-container" class="container-fluid">
    <div class="row">
      <!-- Sidebar with Filter and Categories List -->
      <div class="col-md-3 bg-light p-3">
        <FilterProduct @filterChanged="handleFilterChange" />
        <CategoriesList @categorySelected="filterProductsByCategory" />
      </div>
      <!-- Products Grid -->
      <div id="product-wrap" class="col-md-9 p-3 bg-light">
        <ProductsGrid :products="filteredProducts" />
      </div>
    </div>
  </div>
</template>

<script>
import { products } from "../fake-data";
import ProductsGrid from "../components/ProductsGrid.vue";
import CategoriesList from '../components/CategoriesList.vue';
import FilterProduct from '../components/FilterProduct.vue';

export default {
  name: "ProductsPage",
  components: {
    ProductsGrid,
    CategoriesList,
    FilterProduct
  },
  data() {
    return {
      products,
      filteredProducts: products, // Start with all products displayed
      selectedPriceRange: [2, 50], // Default price range for the filter
    }
  },
  methods: {
    filterProductsByCategory(categoryId) {
      this.filteredProducts = this.products.filter(product => product.categoryId === categoryId);
    },
    handleFilterChange({ priceRange }) {
      this.selectedPriceRange = priceRange;
      this.applyFilters();
    },
    applyFilters() {
      this.filteredProducts = this.products.filter(product => {
        const inPriceRange = product.price >= this.selectedPriceRange[0] && product.price <= this.selectedPriceRange[1];
        return inPriceRange;
      });
    }
  }
}
</script>

<style scoped>
#product-wrap {
  min-height: 100vh;
  padding-left: 10px; /* Ensure even padding on both sides */
}

#page-wrap-container {
  height: 100vh;
  padding-right: 20px; /* Ensure even padding on both sides */
}

.bg-light {
  background-color: #f8f9fa !important;
}

/* Ensure consistent width for Filter and Categories List */
.col-md-3 {
  max-width: 100%; /* Adjust max-width to keep both components the same width */
}
</style>
