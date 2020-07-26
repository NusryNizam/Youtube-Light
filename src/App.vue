<template>
  <div id="app">
    <Product />
  </div>
</template>

<script>
import Product from "./components/Product.vue";

export default {
  name: "App",
  components: {
    Product,
  },
};
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0;
  padding: 0;
}
</style>
