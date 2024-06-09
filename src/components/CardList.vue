<script setup>
  import Card from "@/components/Card.vue";


  defineProps({
    items: Array,
    addToFavorite: Function
  })

const emit = defineEmits(['addToFavorite', 'addToCart'])

</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
        v-for="item in items"
        :key="item.id"
        :id="item.id"
        :price="item.price"
        :title="item.title"
        :imageUrl="item.imageUrl"
        :isFavorite="item.isFavorite"
        :onClickFavorite="() => emit('addToFavorite', item)"
        :onClickAdd="() => emit('addToCart', item)"
        :isAdded="item.isAdded"
        />
  </div>
</template>
