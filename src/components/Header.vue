<script setup>

  defineProps({
    totalPrice: Number
  })

  const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header class="flex justify-between border-b border-slate-200 px-10 py-8">
    <div class="flex items-center gap-4">
      <router-link to="/">
        <img src="/logo.png" alt="Logo" class="w-10">
      </router-link>
    </div>
    <div>
      <h2 class="text-xl font-bold uppercase text-slate-400">Vue Sneakers</h2>
      <p class="text-slate-400">Магазин лучших кроссовок</p>
    </div>

    <ul class="flex items-center gap-10">
      <li @click="() => emit('openDrawer')" class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
        <img src="/cart.svg" alt="Cart">
        <b>{{totalPrice}} руб.</b>
      </li>

      <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
        <img src="/heart.svg" alt="Heart">
        <span>Закладки</span>
      </li>

      <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
        <img src="/profile.svg" alt="Profile">
        <span>Профиль</span>
      </li>
    </ul>

  </header>
</template>