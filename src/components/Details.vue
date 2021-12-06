<script>
import { defineComponent } from "vue";

export default defineComponent({
  props: {
    details: {
      type: Array,
      required: true,
    },
  },
});
</script>

<template>
  <ul>
    <li v-for="detail in details">{{ detail }}</li>
  </ul>
</template>
