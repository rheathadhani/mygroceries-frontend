<template>
  <div id="page-wrap" class="container mt-2 bg-light p-4 rounded">
    <h3 class="pt-1 pb-0 text-center">Shopping Cart</h3>
    <div class="card shadow-sm">
      <!-- Card Body for Product List and Total -->
      <div class="card-body pt-2 ">
        <ProductsList :products="cartItems" @updateQuantity="updateQuantity" @removeProduct="removeFromCart"></ProductsList>

        <!-- Total Price -->
        <h3 id="total-price" class="text-end mt-4">Total: ${{ totalPrice }}</h3>
        
        <!-- Checkout Button -->
        <router-link to="/checkout" id="checkout-link">
          <button id="checkout-button" class="btn btn-dark w-100 mt-3">Proceed to Checkout</button>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { cartItems } from '../fake-data';
import ProductsList from "../components/ProductsList.vue";

export default {
  name: 'CartPage',
  components: {
    ProductsList,
  },
  data() {
    return {
      cartItems: cartItems.map(item => ({ ...item, quantity: 1 })), // Initialize with quantity
    }
  },
  computed: {
    totalPrice() {
      return this.cartItems.reduce(
        (sum, item) => sum + Number(item.price) * item.quantity,
        0,
      );
    }
  },
  methods: {
    updateQuantity(productId, newQuantity) {
      const product = this.cartItems.find(item => item.id === productId);
      if (product) {
        product.quantity = newQuantity;
      }
    },
    removeFromCart(productId) {
      this.cartItems = this.cartItems.filter(item => item.id !== productId);
    }
  }
};
</script>

<style scoped>
#total-price {
  padding: 16px 0;
  text-align: right;
}

#checkout-button {
  width: 100%;
  transition: background-color 0.3s, color 0.3s;
}

.product-container {
  align-content: center;
  border-bottom: 1px solid #ddd;
  display: flex;
  padding: 16px;
  width: 100%;
}

.product-image {
  flex: 1;
  height: 100px;
  max-width: 100px;
}

.details-wrap {
  padding: 0 16px;
  flex: 3;
}

.remove-button {
  flex: 1;
  margin: auto;
}
</style>
