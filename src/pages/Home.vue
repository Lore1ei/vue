<script setup>
import CardList from "@/components/CardList.vue";
import debounce from 'lodash.debounce'
import axios from "axios";
import {inject, onMounted, reactive, ref, watch} from "vue";
  defineProps({

  })

const {addToCart, removeFromCart} = inject('cart')
const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const addToFavorite = async (item) => {
  try{
    if(!item.isFavorite){
      const obj = {
        parentId: item.id
      }
      item.isFavorite = true;
      const {data} = await axios.post(`https://d1fa7c08d642262a.mokky.dev/favorites`, obj)
      console.log(data)
      item.isFavoriteId = data.id
    }else{
      item.isFavorite = false
      await axios.delete(`https://d1fa7c08d642262a.mokky.dev/favorites/${item.favoriteId}`)
    }
  }catch (e){
    console.log(e)
  }
}

const onChangeSelect = event => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 300)

const onClickAddPlus = (item) => {
  if(!item.isAdded){
    addToCart(item)
  }else{
    removeFromCart(item)
  }

}
const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    }

    if(filters.searchQuery){
      params.title = `*${filters.searchQuery}*`
    }

    const {data} = await axios.get(`https://d1fa7c08d642262a.mokky.dev/items`, {
      params
    })
    items.value = data.map(obj => ({
      ...obj,
      isFavoriteId: null,
      isFavorite: false,
      isAdded: false,
    }))
  } catch (err) {
    console.log(err)
  }
}

const fetchFavorites = async () => {
  try {
    const {data: favorites} = await axios.get(`https://d1fa7c08d642262a.mokky.dev/favorites`)
    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.parentId === item.id)

      if(!favorite){
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    })
  } catch (err) {
    console.log(err)
  }
}
onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})



watch(filters, fetchItems)
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

    <div class="flex gap-4">
      <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
        <option value="name">По названию</option>
        <option value="price">По цене (Дешевые)</option>
        <option value="-price">По цене (Дорогие)</option>
      </select>

      <div class="relative">
        <img src="/search.svg" alt="" class="absolute left-3 top-3">
        <input @input="onChangeSearchInput"
               type="text" class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
               placeholder="Поиск..."
        >
      </div>
    </div>
  </div>

  <div class="mt-10">
    <CardList :items="items" @addToFavorite="addToFavorite" @add-to-cart="onClickAddPlus"/>
  </div>
</template>
