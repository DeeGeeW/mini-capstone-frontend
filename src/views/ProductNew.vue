<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newProduct: {},
      errors: [],
    };
  },
  methods: {
    createProduct() {
      console.log(this.newProduct);
      axios
        .product("/products", this.newProduct)
        .then((response) => {
          console.log(response);
          this.$router.push("/products");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(this.errors);
        });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>New product</h1>
    <form v-on:submit.prevent="createProduct()">
      <ul>
        <li v-for="error in errors" v-bind:key="error" style="color: red">{{ error }}</li>
      </ul>
      <p>
        name:
        <input type="text" v-model="newProduct.name" />
      </p>
      <p>
        price:
        <input type="text" v-model="newProduct.price" />
      </p>
      <p>
        Images:
        <input type="text" v-model="newProduct.images" />
      </p>
      <p>
        description:
        <input type="text" v-model="newProduct.description" />
      </p>
      <p>
        Supplier_id:
        <input type="text" v-model="newProduct.supplier_id" />
      </p>
      <input type="submit" value="Create Product" />
    </form>
  </div>
</template>

<style></style>
