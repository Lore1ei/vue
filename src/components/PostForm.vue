<template>
  <form @submit.prevent>
    <h4>Создание поста</h4>
    <my-input v-model="post.title"
           type="text"
           placeholder="Название поста" />
    <my-input v-model="post.body"
           type="text"
           placeholder="Описание поста" />
    <MyButton
        class="btn"
        type="submit"
        style="align-self: flex-end; margin-top: 15px"
        @click="createPost">Создать</MyButton>
  </form>
</template>

<script>
import MyInput from "@/components/UI/MyInput";
export default {
  components: {MyInput},
  data(){
    return{
      post: {
        title: '',
        body: '',
      }
    }
  },
  methods: {
    createPost(){
      this.post.id = Date.now();
      this.$emit('create', this.post, 'sad', 'sad')
      this.post = {
        title: '',
        body: ''
      }
    }
  }
}
</script>

<style scoped>
  form{
    display: flex;
    flex-direction: column;
  }
</style>