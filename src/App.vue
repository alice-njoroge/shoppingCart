<script setup>
import {onMounted, ref} from "vue";
import HeaderComponent from "@/components/HeaderComponent.vue";
import ProductsList from "@/components/ProductsList.vue";

const products = ref([])

onMounted(async () => {
  const data = await fetch('https://dummyjson.com/products');
  const response = await data.json()
  products.value = response.products


})


</script>
<template>
<HeaderComponent/>
  <main>
    <div class="container m-auto w-full justify-center flex flex-wrap">
      <ProductsList v-for="product in products" :key="product.id" :product="product"/>
    </div>
  </main>
</template>

<style scoped>

</style>
