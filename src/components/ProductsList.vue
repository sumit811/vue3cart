<template>
    <section class="products-outer">
        <h2>Products</h2>
        <div v-if="loading" class="loading">Loading products.....</div>
        <div v-else-if="error" class="error">{{ error }}</div>
        <div v-else class="product-inner">
            <article v-for="product in products.products" :key="product.id" class="product-card">
                <figure class="image-wrapper">
                    <img :src="product.thumbnail" :alt="product.title" />
                    <span v-if="product.discountPercentage" class="discount-badge">
                        -{{ product.discountPercentage }}
                    </span>
                </figure>
                <h3 class="product-title">
                    <router-link :to="`/product/${product.id}`">
                        {{ product.title }}
                    </router-link>
                </h3>
            </article>
        </div>
    </section>
</template>
<script setup>
import { ref, onMounted } from 'vue';
import apiClient from '@/api/services/axios';

const products = ref([]);
const loading = ref(true);
const error = ref(null);

const fetchProducts = async () => {
    try {
        const response = await apiClient.get('APIdata/products.json');
        products.value = response.data;

    } catch (err) {
        error.value = err.response?.data?.message || err.message || 'Something went wrong.';
    } finally {
        loading.value = false;
    }
};
fetchProducts();
// onMounted(() => {
//   fetchProducts();
// });
</script>
<style lang="scss" scoped>
$primary-color: #42b883;
$text-dark: #2c3e50;
$text-muted: #7f8c8d;
$bg-card: #ffffff;
$border-radius: 12px;
$shadow: 0 4px 16px rgba(0, 0, 0, 0.08);

.products-outer {
    padding: 2rem;
    max-width: 1200px;
    margin: 0 auto;

    h2 {
        text-align: center;
        color: $text-dark;
        margin-bottom: 2rem;
        font-size: 2rem;
    }
}

.loading,
.error {
    text-align: center;
    font-size: 1.2rem;
    padding: 3rem;
    color: $text-muted;
}

.error {
    color: #e74c3c;
}

.product-inner {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 2rem;
}

.product-card {
    background: $bg-card;
    border-radius: $border-radius;
    overflow: hidden;
    box-shadow: $shadow;
    display: flex;
    flex-direction: column;
    transition: transform 0.3s ease, box-shadow 0.3s ease;

    &:hover {
        transform: translateY(-5px);
        box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
    }

    .product-link {
        display: block;
        width: 100%;
    }

    .image-wrapper {
        position: relative;
        background-color: #f9f9f9;
        height: 220px;
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 0;

        img {
            max-height: 100%;
            max-width: 100%;
            object-fit: contain;
            padding: 1rem;
        }

        .discount-badge {
            position: absolute;
            top: 10px;
            left: 10px;
            background: #e74c3c;
            color: #fff;
            padding: 0.25rem 0.5rem;
            border-radius: 4px;
            font-size: 0.85rem;
            font-weight: bold;
        }

    }

    .product-title {
        font-size: 1rem;
        margin: 1rem;
        font-weight: 600;
        line-height: 1.4;
        text-align: center;

        a {
            text-decoration: none;
            color: $text-dark;
            transition: color 0.2s ease;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
            overflow: hidden;
        }
    }
}
</style>
