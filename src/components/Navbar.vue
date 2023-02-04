<template>
  <div class="flex flex-row items-center bg-slate-50">
    <div
      class="cursor-pointer flex ml-4 my-4 flex-row justify-center items-center bg-indigo-400 py-2 p-3 rounded-lg"
    >
      <h1 class="text-5xl font-bold">Ardunic</h1>
      <h2 class="text-xl mb-5 font-medium">Shop</h2>
    </div>
    <div class="text-center w-full">
      <ul
        class="flex flex-row justify-evenly text-xl font-semibold cursor-pointer"
      >
        <li>Home</li>
        <li>Learn</li>
        <li>Blog</li>
        <li>Jobs</li>
        <li>About</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "Navbar",
  data() {
    return {};
  },
  methods: {},
};
</script>
