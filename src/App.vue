<template>
  <div class="fixed w-full z-20 scroll-smooth" ref="wew">
    <Navbar></Navbar>
  </div>
  <div class="grid grid-cols-6 bg-slate-50 text-left">
    <div v-if="isFetched" class="col-span-4 col-start-2 mt-28">
      <img class="rounded-lg" src="../src/assets/web.png" alt="" />
      <div class="grid grid-cols-3 my-8">
        <div
          class="group relative m-4 p-4 rounded-3xl bg-white drop-shadow flex flex-col items-center hover:scale-105 hover:shadow-lg duration-200"
          v-for="product in products"
        >
          <img
            class="max-h-48 cursor-pointer"
            :src="imagePrefix + product.images[0]"
            alt=""
          />
          <div
            class="w-full my-4 bg-stone-800 px-3 py-2 rounded-xl text-lg font-bold text-white flex flex-row items-center justify-center"
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
            class="opacity-0 absolute bottom-0 font-semibold bg-indigo-400 text-white w-full rounded-b-3xl py-4 group-hover:opacity-100 flex flex-row items-center justify-center transition delay-150"
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
</template>

<script>
import Navbar from "../src/components/Navbar.vue";
export default {
  components: {
    Navbar,
  },
  name: "App",
  data() {
    return {
      API: "https://api.ardunic.com/v1/products",
      isFetched: false,
      products: [],
      imagePrefix: "https://ardunic-images.s3.eu-central-1.amazonaws.com/",
      fav: [],
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
    window.addEventListener("scroll", this.handleScroll);
  },
  methods: {
    handleScroll() {
      this.$refs.wew.classList.add("shadow-lg");
      return;
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
  },
};
</script>

<style></style>
