<script setup>
import DrawerHead from "@/components/DrawerHead.vue";
import CartItemList from "@/components/CartItemList.vue";
import InfoBlock from "@/components/InfoBlock.vue";


defineProps({
  totalPrice: Number,
  vatPrice: Number,
  isCreatingOrder: Boolean
})

const emit = defineEmits(['createOrder'])


</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead/>

    <div v-if="!totalPrice" class="flex h-full items-center">
      <InfoBlock title="Корзина пустая" description="Добавьте товар" image-url="/package-icon.png"/>
    </div>




    <div v-else class="flex flex-col gap-4 mt-7">
      <CartItemList/>
      <div class="flex items-end gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"/>
        <span class="font-bold">{{totalPrice}} руб.</span>
      </div>

      <div class="flex items-end gap-2">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed"/>
        <span class="font-bold">{{vatPrice}} руб.</span>
      </div>
      <button
          @click="() => emit('createOrder')"
          :disabled="isCreatingOrder"
          class="flex justify-center cursor-pointer
        disabled:bg-slate-300 items-center gap-3 w-full py-3 mt-4 bg-lime-500 text-white rounded-xl transition active:bg-lime-700 hover:bg-lime-600"
      >
        Оформить заказ
        <img src="/arrow-next.svg" alt="Arrow"/>
      </button>
    </div>

  </div>
</template>
