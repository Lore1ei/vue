<template>
  <div class="navbar">
    <h1 @click="$router.push('/')">Vue3</h1>
    <div class="navbar__btns">
      <my-button @click="$router.push('/posts')">Посты</my-button>
      <my-button @click="$router.push('/about')" style="margin-left: 20px">О сайте</my-button>
      <my-button @click="$router.push('/store')" style="margin-left: 20px">Store</my-button>
    </div>
  </div>
</template>

<script>
import MyButton from "@/components/UI/MyButton";
export default {
  components: {MyButton}
}
</script>

<style scoped>
.navbar{
  height: 50px;
  background: lightblue;
  box-shadow: 2px 2px 4px gray;
  display: flex;
  align-items: center;
  padding: 0 15px;
}
.navbar__btns{
  margin-left: auto;
}
</style>