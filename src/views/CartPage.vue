<template>
  <div id="page-wrap">
    <h1>Shopping Cart</h1>
    <ProductsList :products="cartItems"></ProductsList>
    <h3 id="total-price">Total: ${{ totalPrice }}</h3>
    <router-link to="/checkout" id="checkout-link">
      <button button id="checkout-button">Proceed to checkout</button>
    </router-link>
  </div>
</template>

<script>
import { cartItems } from '../fake-data';
import ProductsList from "../components/ProductsList.vue"

export default {
  name: 'CartPage',
  components: {
    ProductsList,
  },
  data() {
    return {
      cartItems,
    }
  },
  computed: {
    totalPrice() {
      return this.cartItems.reduce(
        (sum, item) => sum + Number(item.price),
        0,
      );
    }
  }
};
</script>

<style scoped>
h1 {
  border-bottom: 1px solid black;
  margin: 0;
  margin-top: 16px;
  padding: 16px;
}

#total-price {
  padding: 16px;
  text-align: right;
}

#checkout-button {
  width: 100%;
  transition: background-color 0.3s, color 0.3s;
  border: 2px solid #000;
}

#checkout-button:hover {
  background-color: #fff;
  color: #000;
}

.product-container {
  align-content: 'center';
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
