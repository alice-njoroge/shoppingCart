<script setup>

import {onMounted} from "vue";

const props = defineProps({
  product: {
    default: () => {
    },
    type: Object,
    required: true
  }
});
const emit = defineEmits({
  add: (product) => {
    return typeof product === 'object';

  }
});
const addToCart = (product) => {
  emit('add', {...product});
}

const carouselDisplay = () => {

}

onMounted(() => {
  setInterval(carouselDisplay, 3000)
})

</script>

<template>

  <div class="card-wrapper">
    <div class=" h-200 object-cover">
      <img class="overflow-hidden h-[200px]" :src="product.thumbnail" alt="">
    </div>
    <div class="h-10 mb-1">
      <h6 class="mb-2 text-2xl font-bold tracking-tight text-blue-900 text-center ">{{ product.title }}</h6>
    </div>
    <div class="h-20">
      <p class="font-normal text-gray-700">{{ product.description }}</p>
    </div>
    <div class="h-5 font-bold flex flex-wrap justify-between">
      <span> Rating: {{ product.rating }} </span>
      <span> Price: {{ product.price }}</span>
      <span> In Stock: {{ product.stock }}</span>
    </div>
    <div class="flex flex-wrap justify-end h-10 my-1">
      <button type="button" @click="addToCart(product)" class="button-wrapper bottom-0 right-0 ">Add to Cart</button>
    </div>
  </div>

</template>

<style scoped>

</style>