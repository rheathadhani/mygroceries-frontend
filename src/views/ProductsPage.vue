<template>
    <div id="page-wrap">
      <ProductsGrid :products = "products" />

    </div>
  </template>


<script>
import { products } from "../fake-data";
import ProductsGrid from "../components/ProductsGrid.vue"

    export default {
        name: "ProductsPage",
        components: {
          ProductsGrid
        },
        data(){
            return {
                products
            }
        }
    }
</script>


<style scoped>
    #page-wrap{
        margin: auto;
        max-width: 800px;
        min-height: 100vh;
    }


  .grid-wrap {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;
    margin-top: 16px;
  }


  /* Responsive Styles */
  @media screen and (min-width: 768px) {
    .product-item {
      width: 30%; /* Adjust the width for larger laptops */
    }
  }

  @media screen and (max-width: 768px) {
    img {
        height: 200px;
        width: 200px;
        margin: 0;
    }
    #grid-wrap {
        align-items: center;
        justify-content: center;
    }
    .product-item {
      width: 90%; /* Make each product item take up the full width on smaller screens */
    }
  }
</style>

 