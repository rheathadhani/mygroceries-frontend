<template>
  <div id="page-wrap-container" class="container-fluid">
    <div class="row">
      <!-- Categories List -->
      <div class="col-md-3 bg-light p-3">
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

export default {
  name: "ProductsPage",
  components: {
    ProductsGrid,
    CategoriesList
  },
  data() {
    return {
      products,
      filteredProducts: products, // Start with all products displayed
    }
  },
  methods: {
    filterProductsByCategory(categoryId) {
      this.filteredProducts = this.products.filter(product => product.categoryId === categoryId);
      console.log(`Category ID: ${categoryId}`);
      console.log('Filtered Products:', this.filteredProducts);
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
</style>
