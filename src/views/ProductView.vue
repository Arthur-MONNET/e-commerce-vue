<template>
  <div class="product">
    <div
      class="
        border-2 border-amber-500
        flex flex-col
        items-stretch
        h-full
        justify-between
        rounded-md
        bg-stone-200
        p-2
        hover:bg-stone-300
      "
      :class="onCart ? 'border-solid' : 'border-none'"
    >
        <img :src="product.image" :title="product.title" class="max-h-64" />
      <div class="my-5">
        <h2>
          <b>{{ product.title }}</b>
        </h2>
        <p>{{ product.price }} €</p>
        <button
          class="rounded mt-3 px-3 py-2"
          :class="!onCart ? 'bg-amber-500' : 'bg-red-500'"
          @click="clickOnProduct()"
        >
          {{ !onCart ? "Add to Cart" : "Remove from Cart" }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from "@/components/HelloWorld.vue";
import axios from "axios";

export default {
  name: "HomeView",
  components: {
    //HelloWorld,
  },
  data: () => {
    return {
      product: {},
      onCart: false,
    };
  },
  mounted() {
    this.initProduct();
  },

  methods: {
    async initProduct(){
        console.log(this.product)
        try {
        const {data} = await axios.get(
          "/product/" + this.$route.params.id
        );
        this.product = JSON.parse(data.product)
        
        this.onCart = data.onCart
      } catch (err) {
        console.log(err);
      }
    },
    clickOnProduct() {
      if (this.onCart) {
        this.onCart = false;
      } else {
        this.onCart = true;
      }
      console.log(this.onCart);
    },
  },
};
</script>
