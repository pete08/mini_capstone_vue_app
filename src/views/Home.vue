<template>
  <div class="home">
    <h1>{{ message }}</h1>

    ---------------------
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
      <p>Id: {{product.id}}</p>
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


    <!-- UPDATE With Modal HERE: -->
    <!--  -->
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Details</h1>
        <p>Name: <input type="text" v-model="currentProduct.name"></p>
        <p>Description: <input type="text" v-model="currentProduct.description"></p>
        <p>what'll it cost: <input type="text" v-model="currentProduct.price"></p>
        <p>Image URL: <input type="text" v-model="currentProduct.image_url"></p>
        <button v-on:click="updateProduct(currentProduct)">Update Product!</button>
        <button>Close</button>
        <button v-on:click="destroyProduct(currentProduct)">Delete Product</button>
      </form>
    </dialog>
    <!-- UPDATE With Modal up there ^ -->

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
    },

    updateProduct: function(product) {
      console.log(product);

      var params = {
        name: this.currentProduct.name ,
        description: this.currentProduct.description ,
        price: this.currentProduct.price ,
        image_url: this.currentProduct.image_url ,
      };

      axios.patch('/api/products/' + this.currentProduct.id, params).then(response => {
        console.log(response.data);
        this.currentProduct = response.data;
      });

    },

    destroyProduct: function(product) {
      console.log(product);
      // delete it in the backend (rails)
      axios.delete('api/products/' + product.id).then(response => {
        console.log(response.data);
        // delete in frontend 
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
        console.log(index);
      });
    }

  }
};
</script>