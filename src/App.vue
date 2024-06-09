<script setup>

import Header from "@/components/Header.vue";
import {computed, provide, ref, watch} from "vue";
import axios from "axios";
import Drawer from "@/components/Drawer.vue";

const items = ref([])
const cart = ref([])
const isCreatingOrder = ref(false)

const drawerOpen = ref(false)

const createOrder = async () => {
  try{
    isCreatingOrder.value = true
    const {data} = await axios.post(`https://d1fa7c08d642262a.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: totalPrice.value
    })
    cart.value = []
    return data
  }catch (err){
    console.log(err)
  }finally {
    isCreatingOrder.value = false
  }
}

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}


const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(item, 1)
  item.isAdded = false
}



watch(cart, () => {
  items.value = items.value.map(item => ({
    ...item,
    isAdded: false
  }))
})


watch(cart, () => {
  localStorage.setItem('cart', JSON.stringify(cart.value))
}, {deep: true})

const totalPrice = computed(
    () => cart.value.reduce((acc, item) => acc + item.price, 0)
)

const vatPrice = computed(
    () => Math.round((totalPrice.value * 5) / 100)
)

const cartIsEmpty =  computed(() => cart.value.length === 0 )

const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value)



provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart
})
</script>

<template>
    <Drawer v-if="drawerOpen" :totalPrice="totalPrice" :vatPrice="vatPrice" @createOrder="createOrder"
            :isCreatingOrder="cartButtonDisabled"/>
  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <Header @open-drawer="openDrawer" :totalPrice="totalPrice"/>

    <div class="p-10">
      <router-view ></router-view>
    </div>
  </div>
</template>

<style scoped>

</style>
