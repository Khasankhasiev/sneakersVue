<template>
  <Drawer v-if="drawerIsOpen" :total-price="totalPrice" :vat-price="vatPrice" />
  <div class="bg-white w-4/5 m-auto mt-14 rounded-xl shadow-xl">
    <Header :total-price="totalPrice" @open-drawer="openDrawer"></Header>
    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, provide, computed } from 'vue'

import Header from '@/components/TheHeader.vue'
import Drawer from '@/components/TheDrawer.vue'

// Корзина (start)
const cart = ref([])

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const drawerIsOpen = ref(false)

const closeDrawer = () => {
  drawerIsOpen.value = false
}

const openDrawer = () => {
  drawerIsOpen.value = true
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart
})

// Корзина (END)
</script>

<style scoped></style>
