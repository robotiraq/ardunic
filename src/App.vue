<template>
  <!-- <PageCompenent></PageCompenent> -->

  <div class="fixed w-full z-20 scroll-smooth" ref="wew">
    <Navbar></Navbar>

    <div
      v-show="addedtocart"
      class="absolute top-0 w-full flex flex-row bg-green-500 p-8 text-2xl font-semibold text-white items-center justify-center"
    >
      <p class="">Added to Cart</p>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="3"
        stroke="currentColor"
        class="w-7 h-7 ml-2"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M4.5 12.75l6 6 9-13.5"
        />
      </svg>
    </div>
  </div>

  <div class="grid grid-cols-6 bg-slate-50 text-left justify-items-center">
    <div v-if="isFetched" class="max-w-7xl col-span-4 col-start-2 mt-28">
      <img
        class="rounded-lg max-w-6xl m-auto mb-3"
        src="../src/assets/web.png"
        alt=""
      />
      <!-- Pagnation -->

      <div
        class="flex flex-row justify-evenly w-1/2 m-auto bg-indigo-400 rounded-lg p-2 text-xl max-w-2xl"
      >
        <button :disabled="currentPage <= 1" @click="currentPage--">
          Previous
        </button>
        <button
          v-for="pageNumber in pages"
          :key="pageNumber"
          class=""
          @click="currentPage = pageNumber"
        >
          {{ pageNumber }}
        </button>
        <button :disabled="currentPage == PageCount" @click="currentPage++">
          Next
        </button>
      </div>

      <!-- Pagnation -->
      <!--           v-for="product in paginatedProducts"
 -->
      <input
        class="border px-4 rounded-lg"
        type="text"
        v-model="searchQuery"
        placeholder="Search products..."
      />
      <div class="grid grid-cols-3 mb-8">
        <div
          class="group relative m-4 p-4 rounded-3xl bg-white drop-shadow flex flex-col items-center hover:shadow-lg duration-200"
          v-for="product in paginatedProducts"
          :key="product.id"
        >
          <img
            class="max-h-52 cursor-pointer"
            :src="imagePrefix + product.images[0]"
            alt=""
          />
          <div
            class="my-4 w-full bg-stone-800 px-3 py-2 rounded-xl text-lg font-bold text-white flex flex-row items-center justify-center"
          >
            <p>{{ product.price.showPrice }}</p>
            <P class="ml-2">IQD</P>
          </div>
          <div
            v-show="product.stock < 15"
            class="bg-red-600 text-white px-2 font-semibold py-2 rounded-l-lg bg-opacity-90 absolute top-20 right-0 text-center"
          >
            {{ product.stock }} Pcs Left
          </div>
          <button
            class="text-lg font-semibold whitespace-pre-line text-stone-800"
          >
            {{ product.title }}
          </button>
          <div
            @click="setFavorite(product._id)"
            class="absolute top-20 left-0 bg-indigo-400 bg-opacity-50 px-1 rounded-r-lg text-white cursor-pointer"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1"
              stroke="currentColor"
              class="w-10 h-10"
              :class="
                fav.some((e) => e === product._id)
                  ? 'fill-red-400 duration-300 '
                  : ''
              "
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M21 8.25c0-2.485-2.099-4.5-4.688-4.5-1.935 0-3.597 1.126-4.312 2.733-.715-1.607-2.377-2.733-4.313-2.733C5.1 3.75 3 5.765 3 8.25c0 7.22 9 12 9 12s9-4.78 9-12z"
              />
            </svg>
          </div>
          <div
            @click="addedtoCart()"
            class="opacity-0 cursor-pointer absolute bottom-0 font-semibold bg-indigo-400 text-white w-full rounded-b-3xl py-4 group-hover:opacity-100 flex flex-row items-center justify-center transition"
          >
            <p>Add to Cart</p>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-6 h-6 ml-3"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M2.25 3h1.386c.51 0 .955.343 1.087.835l.383 1.437M7.5 14.25a3 3 0 00-3 3h15.75m-12.75-3h11.218c1.121-2.3 2.1-4.684 2.924-7.138a60.114 60.114 0 00-16.536-1.84M7.5 14.25L5.106 5.272M6 20.25a.75.75 0 11-1.5 0 .75.75 0 011.5 0zm12.75 0a.75.75 0 11-1.5 0 .75.75 0 011.5 0z"
              />
            </svg>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- <pagination
    :totalPages="10"
    :perPage="10"
    :currentPage="currentPage"
    @pagechanged="onPageChange"
  ></pagination> -->
</template>

<script>
import Navbar from "../src/components/Navbar.vue";
import Pagination from "./components/Pagination.vue";
import PageCompenent from "./components/Page.vue";
export default {
  components: {
    Navbar,
    Pagination,
    PageCompenent,
  },
  name: "App",
  data() {
    return {
      API: "https://api.ardunic.com/v1/products?s=100",
      isFetched: false,
      products: [],
      imagePrefix: "https://ardunic-images.s3.eu-central-1.amazonaws.com/",
      fav: [],
      addedtocart: false,
      // currentPage: 1,
      perPage: 6,
      currentPage: 1,
      PageCount: 0,
      searchQuery: "",
    };
  },
  created() {
    fetch(this.API)
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.products = data.data;
        this.isFetched = true;
      });
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
  },
  computed: {
    filteredProducts() {
      return this.products.filter((product) =>
        product.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
    pageCount() {
      this.PageCount = Math.ceil(this.products.length / this.perPage);
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
    onPageChange(page) {
      console.log(page);
      this.currentPage = page;
    },
    handleScroll() {
      if (window.pageYOffset > 1) {
        this.$refs.wew.classList.add("shadow-lg");
      } else {
        this.$refs.wew.classList.remove("shadow-lg");
      }
    },
    setFavorite(favMovie) {
      let index = this.fav.indexOf(favMovie);
      if (this.fav.some((e) => e == favMovie)) {
        this.fav.splice(index, 1);
      } else {
        this.fav.splice(0, 0, favMovie);
      }
      console.log(this.fav);
    },

    addedtoCart() {
      this.addedtocart = true;
      setTimeout(() => {
        this.addedtocart = false;
      }, "1000");
    },
  },
};
</script>

<style></style>
