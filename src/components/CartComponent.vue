<script setup>
import {computed, ref, toRaw, watch} from "vue";

defineEmits({
  'showCart': null,
  'removeItem': (item) => {
    if (!item && typeof item !== 'number'){
      return false
    } else {
      return true
    }
  }
});
const props = defineProps({
  cartItems : {
    type: Array,
    default: () => [],
  }
});

const _cartItems = ref([...props.cartItems]);
watch(
    ()=>props.cartItems,
    ()=>{
      _cartItems.value = [...props.cartItems]
    }
)
const total = computed(() => {
  return _cartItems.value.reduce((acc, item) => acc + (item.price * item.qty), 0)
})
console.log(toRaw(_cartItems.value));


</script>

<template>
  <Transition>
  <div class="relative z-10" aria-labelledby="slide-over-title" role="dialog" aria-modal="true">
    <!--
      Background backdrop, show/hide based on slide-over state.

      Entering: "ease-in-out duration-500"
        From: "opacity-0"
        To: "opacity-100"
      Leaving: "ease-in-out duration-500"
        From: "opacity-100"
        To: "opacity-0"
    -->
    <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>

      <div class="fixed inset-0 overflow-hidden">
        <div class="absolute inset-0 overflow-hidden">
          <div class="pointer-events-none fixed inset-y-0 right-0 flex max-w-full pl-10">
            <!--
              Slide-over panel, show/hide based on slide-over state.

              Entering: "transform transition ease-in-out duration-500 sm:duration-700"
                From: "translate-x-full"
                To: "translate-x-0"
              Leaving: "transform transition ease-in-out duration-500 sm:duration-700"
                From: "translate-x-0"
                To: "translate-x-full"
            -->
            <div class="pointer-events-auto w-screen max-w-md">
              <div class="flex h-full flex-col overflow-y-scroll bg-white shadow-xl">
                <div class="flex-1 overflow-y-auto px-4 py-6 sm:px-6">
                  <div class="flex items-start justify-between">
                    <h2 class="text-lg font-medium text-gray-900" id="slide-over-title">Shopping cart</h2>
                    <div class="ml-3 flex h-7 items-center">
                      <button @click="$emit('showCart', false )" type="button"
                              class="relative -m-2 p-2 text-gray-400 hover:text-gray-500">
                        <span class="absolute -inset-0.5"></span>
                        <span class="sr-only">Close panel</span>
                        <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
                             aria-hidden="true">
                          <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12"/>
                        </svg>
                      </button>
                    </div>
                  </div>

                  <div class="mt-8">
                    <div v-if="!_cartItems.length">
                      Your cart items will be displayed here when you start shopping :)
                    </div>
                    <div class="flow-root" v-else>
                      <ul role="list" class="-my-6 divide-y divide-gray-200">
                        <li class="flex py-6" v-for="item in _cartItems">
                          <div class="h-24 w-24 flex-shrink-0 overflow-hidden rounded-md border border-gray-200">
                            <img :src="item.thumbnail" alt="" class="h-full w-full object-cover object-center">
                          </div>

                          <div class="ml-4 flex flex-1 flex-col">
                            <div>
                              <div class="flex justify-between text-base font-medium text-gray-900">
                                <h3>
                                  {{ item.title }}
                                </h3>
                                <p class="ml-4">{{ `$${Number(item.price * item.qty).toFixed(2)}` }}</p>
                              </div>
                              <p class="mt-1 text-sm text-gray-500">{{ item.category }}</p>
                            </div>
                            <div class="flex flex-1 items-end justify-between text-sm">
                              <p class="text-gray-500"> Qty: {{ item.qty }}</p>

                              <div class="flex">
                                <button type="button" class="font-medium text-indigo-600 hover:text-indigo-500"
                                        @click="$emit('removeItem', item.id)">Remove
                                </button>
                              </div>
                            </div>
                          </div>
                        </li>
                      </ul>
                    </div>
                  </div>
                </div>

                <div class="border-t border-gray-200 px-4 py-6 sm:px-6">
                  <div class="flex justify-between text-base font-medium text-gray-900" v-if="_cartItems.length > 0">
                    <p>Subtotal</p>
                    <p>{{ Number(total).toFixed(2) }}</p>
                  </div>
                  <p class="mt-0.5 text-sm text-gray-500" v-if="_cartItems.length > 0">Shipping and taxes calculated at
                    checkout.</p>
                  <div class="mt-6" v-if="_cartItems.length > 0">
                    <a href="#"
                       class="flex items-center justify-center rounded-md border border-transparent bg-indigo-600 px-6 py-3 text-base font-medium text-white shadow-sm hover:bg-indigo-700">Checkout</a>
                  </div>
                  <div class="mt-6 flex justify-center text-center text-sm text-gray-500">
                    <p>
                      or
                      <button @click="$emit('showCart', false)" type="button"
                              class="font-medium text-indigo-600 hover:text-indigo-500">
                        Continue Shopping
                        <span aria-hidden="true"> &rarr;</span>
                      </button>
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </Transition>

</template>
<style scoped>
.p-enter-active,
.p-leave-active {
  transition: opacity 500ms ease-in-out
}

.p-enter-from,
.p-leave-to {
  opacity: 0;
}


</style>
