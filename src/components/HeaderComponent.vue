<script setup>
defineEmits(['showCart']);
</script>
<template>
  <header>
    <div class="header-wrapper mb-2">
      <div class="flex flex-wrap justify-between mx-20">
        <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 "> Tech Products at Awesome Prices :)</h5>
        <button class="button-wrapper" @click="$emit('showCart', true)"> Open Cart</button>
      </div>
    </div>
  </header>
</template>
