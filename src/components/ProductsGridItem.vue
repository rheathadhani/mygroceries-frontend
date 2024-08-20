<template>
  <div class="product-item card" @click="goToProductPage">
    <div class="card-img-wrapper d-flex align-items-center justify-content-center">
      <img v-bind:src="product.imageUrl" class="card-img-top" />
    </div>
    <div class="card-body text-center">
      <h5 class="product-name card-title">{{ product.name }}</h5>
      <p class="product-price card-text">${{ product.price }}</p>
      <div class="quantity-controls">
        <button v-if="!showQuantityControls" id="quantity" class="btn btn-dark" @click.stop="addToCart"> + </button>
        <div v-else>
          <button id="quantity" class="btn btn-dark" @click.stop="increaseQuantity"> + </button>
          <input type="text" v-model="quantity" id="quantity-input" class="form-control d-inline-block text-center" disabled />
          <button id="quantity" class="btn btn-dark" @click.stop="decreaseQuantity"> - </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductsGridItem',
  props: ['product'],
  data() {
    return {
      quantity: 1, // Default quantity
      showQuantityControls: false, // Controls whether the minus and input should be shown
    };
  },
  methods: {
    increaseQuantity() {
      this.quantity += 1;
    },
    decreaseQuantity() {
      if (this.quantity > 1) {
        this.quantity -= 1;
      }
    },
    addToCart() {
      this.showQuantityControls = true;
      this.$emit('add-to-cart', {
        product: this.product,
        quantity: this.quantity
      });
    },
    goToProductPage() {
      this.$router.push('/products/' + this.product.id);
    }
  }
}
</script>

<style scoped>
.product-item {
  border-radius: 8px;
  box-shadow: 0px 2px 5px #888;
  background-color: #fff; /* Ensure the product item has a white background */
  padding: 15px; /* Reduced padding */
  position: relative;
  cursor: pointer;
  width: calc(100% / 4 - 40px); /* Adjusted width to fit 4 items per row with some margin */
/* Added margin to create space between cards */
}

.card-img-wrapper {
  height: 120px; /* Reduced height for smaller image */
  width: 120px; /* Reduced width for smaller image */
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 8px auto; /* Adjusted margin */
}

img {
  max-height: 100%;
  max-width: 100%;
  object-fit: contain; /* Ensure the image fits within the bounds */
}

.product-name {
  margin-bottom: 4px; /* Adjusted margin */
  font-size: 1rem; /* Reduced font size */
  font-weight: 500; /* Maintain consistent text size and weight */
}

.product-price {
  font-size: 0.9rem; /* Reduced font size */
  margin-bottom: 8px; /* Adjusted margin */
}

.quantity-controls {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 8px; /* Adjusted margin */
}

#quantity {
  padding: 4px 8px; /* Reduced padding for button size */
  border-radius: 5px;
  transition: all ease 0.3s;
}

#quantity:hover {
  transform: scale(1.1);
}

#quantity-input {
  width: 40px; /* Adjusted width for better alignment */
  height: 25px; /* Adjusted height */
  border: 1px solid #000000;
  background-color: #fff;
  margin: 0 4px; /* Adjusted margin */
}
</style>
