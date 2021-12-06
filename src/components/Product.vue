<script>
import { defineComponent } from "vue";
import Details from "./Details.vue";

export default defineComponent({
  components: { Details },
  props: {
    premium: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      brand: "Happy Socks",
      product: "Socks",
      selectedVariant: 0,
      details: ["80% cotton", "20% polyester", "Gender-neutral"],
      onSale: true,
      variants: [
        {
          variantId: 2234,
          variantColor: "green",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
          variantQuantity: 10,
        },
        {
          variantId: 2235,
          variantColor: "blue",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-blue-onWhite.jpg",
          variantQuantity: 0,
        },
      ],
      sizes: [
        {
          sizeId: 2387,
          size: "43-46",
        },
        {
          sizeId: 8374,
          size: "39-42",
        },
      ],
    };
  },
  methods: {
    addToCart() {
      this.$emit("add-to-cart", this.variants[this.selectedVariant].variantId);
    },
    removeFromCart() {
      this.$emit(
        "remove-from-cart",
        this.variants[this.selectedVariant].variantId
      );
    },
    updateProduct(index) {
      this.selectedVariant = index;
      console.log(index);
    },
  },
  computed: {
    title() {
      return this.brand + " " + this.product;
    },
    image() {
      return this.variants[this.selectedVariant].variantImage;
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity;
    },
    onSaleMessage() {
      return this.brand + " " + this.product + " are on sale!";
    },
    shipping() {
      if (this.premium) {
        return "free";
      }
      return 2.99;
    },
  },
});
</script>

<template>
  <div class="product">
    <div class="product-image">
      <img :src="image" />
    </div>

    <div class="product-info">
      <h1>{{ title }}</h1>
      <span v-if="onSale">{{ onSaleMessage }}</span>
      <p
        v-show="inStock"
        v-if="this.variants[this.selectedVariant].variantQuantity > 10"
      >
        In Stock
      </p>
      <p
        v-else-if="
          this.variants[this.selectedVariant].variantQuantity <= 10 &&
          this.variants[this.selectedVariant].variantQuantity > 0
        "
      >
        Almost sold out
      </p>
      <p v-else :class="{ outOfStock: !inStock }">Out of Stock</p>
      <p>Shipping: {{ shipping }}</p>

      <Details :details="details"></Details>

      <div
        v-for="(variant, index) in variants"
        :key="variant.variantId"
        class="color-box"
        :style="{ backgroundColor: variant.variantColor }"
        @mouseover="updateProduct(index)"
      ></div>

      <ul>
        <li v-for="size in sizes" :key="sizes.sizeId">{{ size.size }}</li>
      </ul>

      <button
        v-on:click="addToCart"
        :disabled="!inStock"
        :class="{ disabledButton: !inStock }"
      >
        Add to cart
      </button>
      <button
        @click="removeFromCart"
        :disabled="cart < 1"
        :class="{ disabledButton: cart < 1 }"
      >
        Remove from cart
      </button>
    </div>
  </div>
</template>
