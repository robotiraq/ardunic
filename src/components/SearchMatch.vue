<template>
  <div id="app" class="text-center">
    <img src="./assets/logo.png" />
    <div class="">
      Search: <input type="text" v-model="query" placeholder="Search" />
    </div>
    <br />
    <div v-html="highlight()"></div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      query: "",
      content: "check if search matches",
    };
  },
  methods: {
    highlight() {
      if (!this.query) {
        return this.content;
      }
      return this.content.replace(new RegExp(this.query, "i"), (match) => {
        return "<strong>" + match + "</strong>";
      });
    },
  },
};
</script>
