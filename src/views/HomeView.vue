<script>
import axios from "axios";
export default {
  data: function () {
    return {
      products: [],
      newProductParams: {},
      editProductParams: {},
      currentProduct: {},
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
    createProduct: function () {
      axios
        .post("/products/create", this.newProductParams)
        .then((response) => {
          console.log("products create", response);
          this.products.push(response.data);
          this.newProductParams = {};
        })
        .catch((error) => {
          console.log("products create error", error.response);
        });
    },
    showProduct: function (product) {
      this.currentProduct = product;
      this.editProductParams = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function (product) {
      axios
        .patch("/products/" + product.id, this.editProductParams)
        .then((response) => {
          console.log("products update", response);
          this.currentProduct = {};
        })
        .catch((error) => {
          console.log("products update error", error.response);
        });
    },
    destroyProduct: function (product) {
      axios.delete("/products/" + product.id).then((response) => {
        console.log("products destroy", response);
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>Welcome to Tarkir!!</h1>
    <img
      src="data:image/jpeg;base64,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"
      alt=""
    />
    <!-- <h1>New product</h1>
    <div>
      Name:
      <input type="text" v-model="newProductParams.name" />
      price:
      <input type="text" v-model="newProductParams.price" />
      Image_url:
      <input type="text" v-model="newProductParams.image_url" />
      description:
      <input type="text" v-model="newProductParams.description" />
      Supplier_id:
      <input type="text" v-model="newProductParams.supplier_id" />
      <button v-on:click="createProduct()">Create product</button>
    </div>
    <h1>All products</h1>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.images[product.images.length - 1].url" v-bind:alt="product.name" />
      <p>price: {{ product.price }}</p>
      <p>description: {{ product.description }}</p>
      <button v-on:click="showProduct(product)">More Info</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product info</h1>
        <p>
          Name:
          <input type="text" v-model="editProductParams.name" />
        </p>
        <p>
          price:
          <input type="text" v-model="editProductParams.price" />
        </p>
        <p>
          Image:
          <input type="text" v-model="editProductParams.image" />
        </p>
        <p>
          description:
          <input type="text" v-model="editProductParams.description" />
        </p>
        <p>
          supplier_id:
          <input type="text" v-model="editProductParams.supplier_id" />
        </p>
        <button v-on:click="updateProduct(currentProduct)">Update</button>
        <button v-on:click="destroyProduct(currentProduct)">Destroy Product</button>
        <button>Close</button>
      </form>
    </dialog> -->
  </div>
</template>
<style>
body {
  text-align: center;
}
</style>
