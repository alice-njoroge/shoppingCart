<script setup>

const props = defineProps({
  product: {
    default: () => {},
    type: Object,
    required: true
  }
})

</script>

<template>

  <div class="card-wrapper">
    <h6 class="mb-2 text-2xl font-bold tracking-tight text-blue-900 text-center ">{{product.category}}</h6>
    <div class="h-18">
      <p class="font-normal text-gray-700">{{product.description}}</p>
    </div>
    <div class="h-5 font-bold flex flex-wrap justify-between">
      <span> Rating: {{product.rating}} </span>
      <span> Price: {{product.price}}</span>
      <span> Price: {{product.stock}}</span>
    </div>
    <div class="flex flex-wrap justify-end h-10 my-1">
      <button type="button" class="button-wrapper bottom-0 right-0 ">Add to Cart</button>
    </div>
  </div>

</template>

<style scoped>

</style>