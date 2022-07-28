<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "WELCOME to my broken app! :)",
      products: [],
      newProductParams: {},
      currentProduct: {},
      editProductParams: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products.json").then((response) => {
        this.products = response.data;
        console.log("All Products:", this.products);
      });
    },
    createProduct: function () {
      axios
        .post("http://localhost:3000/products.json", this.newProductParams)
        .then((response) => {
          console.log("Success!", response.data);
          this.product.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showProduct: function (product) {
      console.log(product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function (product) {
      axios.patch("http://localhost:3000/products/" + product.id + ".json", this.editProductParams).then((response) => {
        console.log("Success!", response.data);
      });
    },
    destroyProduct: function (product) {
      axios.delete("http://localhost:3000/products/" + product.id).then((response) => {
        console.log("Success!", response.data);
        var index = this.products.indexOf(product);
        this.recipes.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>New Product</h2>
    <div>
      Name
      <input type="text" v-model="newProductParams.name" />
      Description
      <input type="text" v-model="newProductParams.description" />
      Price
      <input type="text" v-model="newProductParams.price" />
      Image URL
      <input type="text" v-model="newProductParams.image_url" />
    </div>
    <button v-on:click="createProduct()">Create Product</button>
    <h2>All Products</h2>
    <div v-for="product in products" v-bind:key="product.id">
      <h4>{{ product.title }}</h4>
      <img :src="product.image_url" alt="product.name" />
      <p>{{ product.name }}</p>
      <button v-on:click="showProduct(product)">More Info!</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info!</h1>
        <p>
          Name:
          <input type="text" v-model="editProductParams.name" />
        </p>
        <p>
          Description:
          <input type="text" v-model="editProductParams.description" />
        </p>
        <p>
          Price:
          <input type="text" v-model="editProductParams.price" />
        </p>
        <!-- <p>Image URL: {{ currentProduct.image_url }}</p> -->
        <button v-on:click="updateProduct(currentProduct)">Update</button>
        <button v-on:click="destroyProduct(currentProduct)">Delete</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style>
h1 {
  color: blue;
}

h2 {
  color: crimson;
}

p {
  color: indigo;
}
</style>
