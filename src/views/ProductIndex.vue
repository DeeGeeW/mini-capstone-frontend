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
        .product("/products/create", this.newProductParams)
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
  <div class="home row">
    <!-- <h1>New product</h1> -->
    <!-- <div>
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
    </div> -->
    <h1>All products</h1>
    <div
      class="col"
      v-bind:class="{ selected: product === currentProduct }"
      v-for="product in products"
      v-bind:key="product.id"
      v-on:mouseover="currentProduct = product"
    >
      <div class="card" style="width: 18rem">
        <img v-bind:src="product.images[0].url" class="card-img-top" v-bind:alt="product.name" />
        <div class="card-body">
          <h5 class="card-title">{{ product.name }}</h5>
          <!-- <p class="card-text">Body: {{ product.body }}</p> -->
          <!-- <a v-bind:href="`/products/${product.id}`" class="btn btn-primary">More Info</a> -->
        </div>
        <button v-on:click="showProduct(product)">More Info</button>
      </div>
      <h2>{{ product.name }}</h2>
      <!-- [product.images.length - 1] -->
      <!-- <img v-bind:src="product.images[product.images.length - 1].url" v-bind:alt="product.name" />
      <p>price: {{ product.price }}</p>
      <p>description: {{ product.description }}</p>
      <button v-on:click="showProduct(product)">More Info</button> -->
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
          <br />
          {{ editProductParams.description }}
        </p>
        <p>
          supplier_id:
          <input type="text" v-model="editProductParams.supplier_id" />
        </p>
        <button v-on:click="updateProduct(currentProduct)">Update</button>
        <button v-on:click="destroyProduct(currentProduct)">Destroy Product</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>
<style>
.selected .card-body {
  color: orangered;
  background-color: aqua;
}
img {
  /* height: 250px; */
  object-fit: cover;
}
</style>
