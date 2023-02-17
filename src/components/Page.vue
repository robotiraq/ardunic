<template>
  <div>
    <div v-for="product in paginatedProducts" :key="product._id">
      <h2>{{ product.title }}</h2>
      <p>{{ product.description }}</p>
    </div>
    <div class="pagination">
      <button v-if="currentPage > 1" @click="currentPage--">Previous</button>
      <button
        v-for="pageNumber in pages"
        :key="pageNumber"
        :class="{ active: currentPage === pageNumber }"
        @click="currentPage = pageNumber"
      >
        {{ pageNumber }}
      </button>
      <button v-if="currentPage < pageCount" @click="currentPage++">
        Next
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
      perPage: 10,
      currentPage: 1,
    };
  },
  computed: {
    pageCount() {
      return Math.ceil(this.products.length / this.perPage);
    },
    paginatedProducts() {
      const startIndex = (this.currentPage - 1) * this.perPage;
      const endIndex = startIndex + this.perPage;
      const productsCopy = [...this.products]; // Make a copy of this.products
      return productsCopy.slice(startIndex, endIndex);
    },
    pages() {
      const pages = [];
      for (let i = 1; i <= this.pageCount; i++) {
        pages.push(i);
      }
      return pages;
    },
  },

  methods: {
    async fetchProducts() {
      try {
        const response = await fetch(
          "https://api.ardunic.com/v1/products?s=100"
        );
        const data = await response.json();
        console.log("data:", data);
        this.products = data.data;
        console.log("this.products:", this.products);
      } catch (error) {
        console.error(error);
      }
    },
  },
  async created() {
    await this.fetchProducts();
  },
};
</script>
