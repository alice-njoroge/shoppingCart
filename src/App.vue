<script setup>
import {onMounted, ref} from "vue";
import HeaderComponent from "@/components/HeaderComponent.vue";
import ProductsList from "@/components/ProductsList.vue";
import CartComponent from "@/components/CartComponent.vue";

const products = ref([])

onMounted(async () => {
  const data = await fetch('https://dummyjson.com/products');
  const response = await data.json()
  products.value = response.products
});
const addToCart = (product) => {
  console.log("adding to cart: ", product);
}
const showCart = ref(false);

</script>
<template>
  <HeaderComponent @show-cart="showCart=$event"/>
  <main>
    <div class="container m-auto w-full justify-center flex flex-wrap">
      <CartComponent v-if="showCart" @show-cart="showCart=$event"/>
      <ProductsList v-for="product in products" :key="product.id" :product="product" @add="addToCart"/>
    </div>

  </main>
</template>

<style scoped>

</style>
