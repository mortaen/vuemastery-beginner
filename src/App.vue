<script>
import { defineComponent } from "vue";
import Product from "./components/Product.vue";

export default defineComponent({
  components: { Product },
  data() {
    return {
      premium: true,
      cart: [],
    };
  },
  methods: {
    updateCart(id) {
      this.cart.push(id);
    },
    removeFromCart(id) {
      this.cart.find(id);
    },
    removeItem(id) {
      for (let i = this.cart.length - 1; i >= 0; i--) {
        if (this.cart[i] === id) {
          this.cart.splice(i, 1);
        }
      }
    },
  },
});
</script>

<template>
  <div>
    <div class="cart">
      <p>Cart({{ cart.length }})</p>
    </div>
    <Product
      :premium="premium"
      @add-to-cart="updateCart"
      @remove-from-cart="removeItem"
    />
  </div>
</template>

<style></style>
