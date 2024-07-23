<template>
  <div id="page-wrap" v-if="product">
    <div id="img-wrap">
      <img :src="product.imageUrl" />
    </div>
    <div id="product-details" @mouseover="speakProductDetails">
      <h1>{{ product.name }}</h1>
      <h3 id="price">${{ product.price }}</h3>  
      <p>Average rating: {{ product.averageRating }}</p>
      <button id="add-to-cart" @mouseover = "speakButton">Add to Cart</button>
      <h4>Description</h4>
      <p>{{ product.description }}</p>
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
    };
  },
  methods: {
    speakProductDetails() {
      if (!this.product) return;

      const productSpeech= new SpeechSynthesisUtterance();
      productSpeech.text = `Product Name: ${this.product.name}. 
                        Price: ${this.product.price} dollars. 
                        Average rating: ${this.product.averageRating}. 
                        Description: ${this.product.description}`;

      const voices = speechSynthesis.getVoices();
      productSpeech.voice = voices.find(voice => voice.name === 'Google US English') || voices[0];
      speechSynthesis.speak(productSpeech);
    },
    speakButton(){
      if (!this.product) return;
      const button = new SpeechSynthesisUtterance();
      button.text = "Click to Add to cart";
      const voices = speechSynthesis.getVoices();
      button.voice = voices.find(voice => voice.name === 'Google US English') || voices[0];
      speechSynthesis.speak(button);
    }
  }
};
</script>

<style scoped>
#page-wrap {
  margin-top: 16px;
  padding: 16px;
  max-width: 600px;
}

#img-wrap {
  text-align: center;
}

img {
  width: 400px;
}

#product-details {
  padding: 16px;
  position: relative;
}

#add-to-cart {
  width: 100%;
}

#price {
  position: absolute;
  top: 24px;
  right: 16px;
}

p {
  text-align: justify;
}

@media screen and (max-width: 768px) {
  img {
    height: 300px;
    width: 300px;
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
