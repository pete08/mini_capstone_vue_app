<template>
  <div class="home">
  <h1>{{ message }}</h1>

    ---------------------
    <br>
    <button v-on:click="indexRecipes()">Get some data...maybe</button>
    <br>
    <button v-on:click="indexProducts()">Get some product data from the api</button>
    <br>
    <button v-on:click="createProduct()">Get it! Make IT!</button>
    <br>
    <!-- <p>name: <input type="text" v-model="name"></p>
    <p>description: <input type="text" v-model="description"></p>
    <p>price: <input type="text" v-model="price"></p>
    <p>image url: <input type="text" v-model="image_url"></p> -->
    <hr>
    <div v-for="product in products">
      <br>
      <br>
      <p> name: {{ product.name }} </p>
      <p> description: {{ product.description }} </p>
      <p> price: {{ product.price }} </p>
      <p> img: {{ product.image_url }} </p>
      <button v-on:click="showInfo(product)">Show more info</button>
      <p><img v-bind:src="product.image_url"></p>
      <br>
      <br>
      <hr>
    </div>

    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Stuff</h1>
        <p><b>{{currentProduct.name}}</b></p>
        <p>Description: {{currentProduct.description}}</p>
        <p>It'll cost ya!: {{currentProduct.price}}</p>
        <p>Image URL: {{currentProduct.image_url}}</p>
        <button>Close</button>
      </form>
    </dialog>

<!-- UPDATE With Modal HERE! -->
<!--  -->
    <!-- <dialog id="product-update">
      <form method="dialog">
        <h1>Update this here product!</h1>
        <p><b>{{currentProduct.name}}</b></p>
        <p>Description: <input type="text" v-model="description"></p>
        <p>It'll cost ya!: {{currentProduct.price}}</p>
        <p>Image URL: {{currentProduct.image_url}}</p>
        <button v-on:click="updateProduct"(currentProduct)
        <button>Close</button>
      </form>
    </dialog> -->
<!--  -->

  </div>
</template>

<style>

</style>



<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js! HEllo@",
      products: [],
      currentProduct: {}
    };
  },

  created: function() {
    this.indexProducts();
  },

  methods: {

    indexRecipes: function() {
      console.log("HI THERE!! From console");
    },
    
    indexProducts: function() {
      console.log("You executed the indexProducts by either refreshign page or clicking  event listener button");
      axios.get('api/products').then(response => {
        console.log(response);
        this.products = response.data;
      });
    },



    createProduct: function() {
      console.log("hi from createPRoduct method...");
      
      var params = {
        name: this.name,
        description: this.description,
        price: this.price,
        image_url: this.image_url
      };
      
      axios.post('api/products', params).then(response => {
        this.products.push(response.data);
        console.log("added a new product! now purchase it!");
      });
    },

    showInfo: function(product) {
      console.log(product);
      this.currentProduct = product;
      document.querySelector('#product-details').showModal();
    }

  }
};
</script>