<script>
import { defineComponent } from "vue";

export default defineComponent({
  data() {
    return {
      name: null,
      review: null,
      rating: null,
      recommendation: null,
      errors: []
    };
  },
  methods: {
      onSubmit() {
        this.errors = []
        if(this.name && this.review && this.rating && this.recommendation) {
          let productReview = {
            name: this.name,
            review: this.review,
            rating: this.rating,
            recommendation: this.recommendation
          }
          this.$emit('review-submitted', productReview)
          this.name = null
          this.review = null
          this.rating = null
          this.recommendation = null
        } else {
          if(!this.name) this.errors.push("Name required.")
          if(!this.review) this.errors.push("Review required.")
          if(!this.rating) this.errors.push("Rating required.")
          if(!this.recommendation) this.errors.push("Recommendation required.")
        }
      }
    }
});
</script>

<template>
 <form class="review-form" @submit.prevent="onSubmit">
      
        <p class="error" v-if="errors.length">
          <b>Please correct the following error(s):</b>
          <ul>
            <li v-for="error in errors">{{ error }}</li>
          </ul>
        </p>

        <p>
          <label for="name">Name:</label>
          <input id="name" v-model="name">
        </p>
        
        <p>
          <label for="review">Review:</label>      
          <textarea id="review" v-model="review"></textarea>
        </p>
        
        <p>
          <label for="rating">Rating:</label>
          <select id="rating" v-model.number="rating">
            <option>5</option>
            <option>4</option>
            <option>3</option>
            <option>2</option>
            <option>1</option>
          </select>
        </p>

        <p>Would you recommend this product?</p>
        <label>
          Yes
          <input type="radio" value="Yes" v-model="recommendation"/>
        </label>
        <label>
          No
          <input type="radio" value="No" v-model="recommendation"/>
        </label>
            
        <p>
          <input type="submit" value="Submit">  
        </p>    
      
    </form>
</template>
