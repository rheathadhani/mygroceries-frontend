<template>
  <div v-if="product" class="container-fluid bg-light vh-100">
    <!-- Container Box for Product Details -->
    <div class="row mt-3 justify-content-center">
      <div class="col-md-8">
        <div class="p-3 rounded">
          <!-- Product Name -->
          <div class="row">
            <div class="col-12 mb-3">
              <h3>{{ product.name }}</h3>
            </div>
          </div>

          <div class="row">
            <!-- Product Image -->
            <div class="col-md-5 text-center">
              <img :src="product.imageUrl" class="img-fluid" />
            </div>

            <!-- Description and Add to Cart / Quantity Controls -->
            <div class="col-md-7">
              <h4>Description</h4>
              <p>{{ product.description }}</p>

              <div v-if="!isInCart">
                <!-- Add to Cart Button -->
                <button class="btn btn-dark w-100 mt-2" @click="addToCart" @mouseover="speakButton">
                  Add to Cart
                </button>
              </div>

              <div v-else class="quantity-controls d-flex align-items-center mt-2">
                <!-- Quantity Controls -->
                <button class="btn btn-dark" @click="changeQuantity(-1)" :disabled="quantity <= 1">-</button>
                <span class="mx-3">{{ quantity }}</span>
                <button class="btn btn-dark" @click="changeQuantity(1)">+</button>
              </div>
            </div>
          </div>

          <!-- Nutritional Information -->
          <div class="row mt-4">
            <div class="col-12">
              <h4>Nutritional Information</h4>
              <p>{{ product.nutritionalInfo }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <NotFoundPage v-else />
</template>

<script>
import { products } from '../fake-data';
import NotFoundPage from './NotFoundPage.vue';

export default {
  name: 'ProductDetailPage',
  components: {
    NotFoundPage,
  },
  data() {
    return {
      product: products.find(p => p.id === this.$route.params.id),
      isInCart: false, // Initially, the product is not in the cart
      quantity: 1, // Default quantity is 1
    };
  },
  methods: {
    addToCart() {
      this.isInCart = true; // Set the product as added to cart
      console.log("Product added to cart");
    },
    changeQuantity(amount) {
      const newQuantity = this.quantity + amount;
      if (newQuantity >= 1) {
        this.quantity = newQuantity;
      }
    },
    speakProductDetails() {
      console.log("test");
    },
    speakButton() {
      console.log("Speak button activated");
    },
  },
};
</script>

<style scoped>
.container-fluid {
  padding: 0;
}

img {
  max-width: 100%;
  height: auto;
}

p {
  text-align: justify;
}

.rounded {
  border-radius: 5px;
}

.p-3 {
  padding: 1rem;
}

.quantity-controls {
  display: flex;
  justify-content: center;
  align-items: center;
}

.quantity-controls button {
  margin: 0 10px;
}
</style>
