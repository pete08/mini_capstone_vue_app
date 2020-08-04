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
    <p>name: <input type="text" v-model="name"></p>
    <p>description: <input type="text" v-model="description"></p>
    <p>price: <input type="text" v-model="price"></p>
    <p>image url: <input type="text" v-model="image_url"></p>
    <br>
    <br>
    <hr>
    <div v-for="product in products">
      <p> name: {{ product.name }} </p>
      <p> description: {{ product.description }} </p>
      <p> price: {{ product.price }} </p>
      <p> img: {{ product.image_url }} </p>
      <img v-bind:src="product.image_url">
      <hr>
    </div>
    <br>
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
    }
  }
};
</script>