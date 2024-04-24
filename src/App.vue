<script setup>
import {onMounted, ref} from "vue";
import HeaderComponent from "@/components/HeaderComponent.vue";
import ProductsList from "@/components/ProductsList.vue";
import CartComponent from "@/components/CartComponent.vue";

const products = ref([]);
const cartItems = ref([]);

onMounted(async () => {
  const data = await fetch('https://dummyjson.com/products');
  const response = await data.json()
  products.value = response.products
});
const addToCart = (product) => {
  const index = cartItems.value.findIndex( item => item.id === product.id)
  if (index > -1){
    cartItems.value[index].qty += 1;
  } else {
    cartItems.value.push({...product, qty: 1})
  }
}
const showCart = ref(false);
const removeItem = (id) => {
  cartItems.value = cartItems.value.filter(item => item.id !== id)
}

const increaseByOne = (id) => {
  const index = cartItems.value.findIndex(item => item.id === id)
  cartItems.value[index].qty += 1;
}

const decreaseByOne = (id) => {
  const index = cartItems.value.findIndex(item => item.id === id)
  if (cartItems.value[index].qty >= 0){
    cartItems.value[index].qty -= 1
  };
}

</script>
<template>
  <HeaderComponent class="sticky top-0" @show-cart="showCart=$event"/>
  <main>
    <div class="container m-auto w-full justify-center flex flex-wrap">
      <transition name="p">
      <CartComponent
          v-if="showCart"
          @show-cart="showCart=$event"
          :cart-items="cartItems"
          @remove-item="removeItem"
          @increase="increaseByOne"
          @decrease="decreaseByOne"
      />
      </transition>
      <ProductsList v-for="product in products" :key="product.id" :product="product" @add="addToCart"/>
    </div>

  </main>
</template>

<style scoped>
.p-enter-active,
.p-leave-active {
  transition: opacity 3s ease-in-out
}
.p-enter-from{
  transform: rotateX(50deg);
  opacity: 0;
}
.p-leave-to {
  transform: translateX(-50px);
  opacity: 0;
}

</style>
