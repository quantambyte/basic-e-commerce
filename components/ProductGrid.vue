<template>
  <div class="product-grid-container">
    <div class="product-grid">
      <ProductCard
        v-for="(product, index) in filteredProducts"
        :key="index"
        :product="product"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ProductGrid',
  data() {
    return {
      products: [],
      activeFilter: 'All',
    };
  },
  computed: {
    filteredProducts() {
      if (this.activeFilter === 'All') {
        return this.products;
      }
      return this.products.filter(product => product.category.toLowerCase() === this.activeFilter.toLowerCase());
    },
  },
  async created() {
    await this.fetchProducts();
    this.setFilterFromQuery();
  },
  methods: {
    async fetchProducts() {
      try {
        const response = await axios.get('https://fakestoreapi.com/products');
        this.products = response.data;
      } catch (error) {
        console.error('Error fetching products:', error);
      }
    },
    setFilterFromQuery() {
      const filterFromUrl = this.$route.query.filter || 'All';
      this.activeFilter = filterFromUrl;
    },
  },
  watch: {
    '$route.query.filter': 'setFilterFromQuery',
  },
};
</script>

<style scoped>
.product-grid-container {
  width: 100%;
  padding-block: 16px;
  .product-grid {
    width: 90%;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20px;
    margin-inline: auto;

    @media (max-width: 1024px) {
      grid-template-columns: repeat(3, 1fr);
    }

    @media (max-width: 768px) {
      grid-template-columns: repeat(2, 1fr);
    }

    @media (max-width: 480px) {
      grid-template-columns: 1fr;
    }
  }
}
</style>
