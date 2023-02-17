<!-- App.vue -->
<template>
  <div class="app">
    <input type="text" v-model="searchQuery" placeholder="Search products..." />
    <ul>
      <li v-for="product in filteredProducts" :key="product.id">
        {{ product.name }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      searchQuery: "",
      products: [],
    };
  },
  mounted() {
    // Fetch products from API
    fetch("https://example.com/products")
      .then((response) => response.json())
      .then((data) => {
        this.products = data; // Save products in data
      });
  },
  computed: {
    filteredProducts() {
      return this.products.filter((product) =>
        product.name.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
  },
};
</script>
