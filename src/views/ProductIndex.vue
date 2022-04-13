<script>
import axios from "axios";
export default {
  data: function () {
    return {
      products: [],
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("/products").then((response) => {
        console.log("products index", response);
        this.products = response.data;
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>All products</h1>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.images" v-bind:alt="product.name" />
      <!-- <p>Width: {{ product.width }}</p>
      <p>Height: {{ product.height }}</p> -->
    </div>
  </div>
</template>

<style></style>
