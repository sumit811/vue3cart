<template>
  <div class="product-detail">
    <h1>The quick brown fo jumps over the lazy dog.</h1>
    productId - {{ productId }}
    <productDetails />
  </div>
</template>
<script setup>
import { useRoute } from 'vue-router';
import productDetails from '@/components/productDetails.vue';

const route = useRoute();
const productId = route.params.id;
</script>