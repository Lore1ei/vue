<script setup>

import CartItem from "@/components/CartItem.vue";
import { inject } from "vue";

const { cart, removeFromCart } = inject('cart')

</script>

<template>
  <div class="flex flex-col gap-5 " v-auto-animate>
    <CartItem v-for="item in cart" :key="item.id" :title="item.title" :price="item.price" :img="item.imageUrl"
              @on-click-delete="() => removeFromCart(item)"/>
  </div>
</template>
