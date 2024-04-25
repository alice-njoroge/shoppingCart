<script setup>
import AppModal from "@/components/AppModal.vue";
import {computed, ref} from "vue";

const props = defineProps({
  showModal: {
    type: Boolean,
    default: false
  },
  categories:{
    type: Array,
    default: ()=>[],
    required: true
  }
});

const emit = defineEmits({
  'showModal' : null,
  'addProduct': (product) => {
    if (typeof product !== 'object'){
      return false
    }
  }
});

const form = ref({
  title: '',
  price: 0,
  category: '',
  description: ''
});

const formDirty = ref({
  title: false,
  price: false,
  category: false,
  description: false
});

const showError = computed(()=> {
  return {
    title: formDirty.value.title === true  && form.value.title === '',
    price: formDirty.value.price === true && form.value.price <= 0,
    category: formDirty.value.category === true && form.value.category === ''
  }
});
const handleSubmit = () => {
  emit('addProduct', form.value);
}

</script>

<template>
  <AppModal v-if="showModal" @show-modal="$emit('showModal',false )">
    <template #modalHeader>
      Add Product
    </template>
    <template #modalBody>
      {{showError}}
      <form @submit.prevent="handleSubmit" class="p-4 md:p-5">
        <div class="grid gap-4 mb-4 grid-cols-2">
          <div class="col-span-2">
            <label for="name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Title</label>
            <input
                @blur="formDirty.title = true"
                type="text"
                v-model="form.title"
                name="name"
                id="name"
                class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500"
                placeholder="Type product name"
                required="">
            <div v-if="showError.title" class="text-red-500">title is required</div>
          </div>
          <div class="col-span-2 sm:col-span-1">
            <label for="price" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Price</label>
            <input
                @blur="formDirty.price = true"
                type="number"
                v-model.number="form.price"
                name="price"
                id="price"
                class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500"
                placeholder="$2999"
                required="">
            <div v-if="showError.price"  class="text-red-500"> price must be greater than 0 </div>
          </div>
          <div class="col-span-2 sm:col-span-1">
            <label for="category" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Category</label>
            <select id="category" v-model="form.category" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-500 focus:border-primary-500 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500">
              <option selected="" disabled >Select category</option>
              <option v-for="category in categories" :value="category">{{category}}</option>
            </select>
          </div>
          <div class="col-span-2">
            <label for="description" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Product Description</label>
            <textarea
                v-model="form.description"
                id="description"
                rows="4"
                class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                placeholder="Write product description here"></textarea>
          </div>
        </div>
        <button type="submit" class="text-white inline-flex items-center bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
          <svg class="me-1 -ms-1 w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M10 5a1 1 0 011 1v3h3a1 1 0 110 2h-3v3a1 1 0 11-2 0v-3H6a1 1 0 110-2h3V6a1 1 0 011-1z" clip-rule="evenodd"></path></svg>
          Add new product
        </button>
      </form>
    </template>

  </AppModal>

</template>

<style scoped>

</style>