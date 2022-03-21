<template>
  <div class="home">
    <div class="fixed rounded bg-amber-500 top-3 right-3 px-3 py-1">panier {{cart.length}}</div>
    <div id="grid" class="flex flex-wrap items-stretch justify-evenly w-full">
      <div
        v-for="product in products"
        :key="product.id"
        class="flex items-stretch w-1/4 px-5 my-5"
      >
        <div class="border-2 border-amber-500 flex flex-col items-stretch h-full justify-between rounded-md bg-stone-200 p-2 hover:bg-stone-300"
        :class="cart.includes(product) ? 'border-solid' : 'border-none'">
          <router-link class="flex justify-center bg-white items-center h-full p-2"
          :to="{name:'product', path: '/product/'+product.id, params: {id: product.id, product:JSON.stringify(product),onCart:cart.includes(product)}}">
            <img :src="product.image" :title="product.title" class="max-h-64" />
          </router-link>
          <div class="my-5">
            <h2>
              <b>{{ product.title }}</b>
            </h2>
            <p>{{ product.price }} €</p>
            <button class="rounded mt-3 px-3 py-2"
            :class="!cart.includes(product)?'bg-amber-500':'bg-red-500'"
            @click="clickOnProduct(product)"
            >{{!cart.includes(product)?'Add to Cart':'Remove from Cart'}}</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from "@/components/HelloWorld.vue";
import axios from "axios";

async function getProducts(products) {
  try {
    const res = await axios.get("https://fakestoreapi.com/products");
    res.data.forEach((product) => {
      products.push(product);
    });
  } catch (error) {
    console.error(error);
  }
}

export default {
  name: "HomeView",
  components: {
    //HelloWorld,
  },
  data: () => {
    return {
      products: [],
      cart:[]
    };
  },

  created: function () {
    getProducts(this.products);
    console.log(this.products);
  },

  methods: {
    clickOnProduct(product){
      if(this.cart.includes(product)){
        this.cart.splice(this.cart.indexOf(product),1)

      }else{
        this.cart.push(product)
      }
      console.log(this.cart)
    }
  },
};
</script>
