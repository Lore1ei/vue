<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <input type="text" v-model.number="modificatorValue">
    <my-button style="margin: 15px 0" @click="showDialog">Создать пост</my-button>
    <my-dialog v-model:show="dialogVisible">
      <PostForm @create="createPost"/>
    </my-dialog>
    <PostList :posts="posts" @remove="deletePost"/>
  </div>
</template>

<script>
  import PostForm from "@/components/PostForm";
  import PostList from "@/components/PostList";
  export default {
    components: { PostList, PostForm},
    data(){
      return{
        posts: [
          {id: 1, title: 'Javascript 1', body: 'Описание поста 1'},
          {id: 2, title: 'Javascript 2', body: 'Описание поста 2'},
          {id: 3, title: 'Javascript 3', body: 'Описание поста 3'},
          {id: 4, title: 'Javascript 4', body: 'Описание поста 4'},
        ],
        dialogVisible: false,
        modificatorValue: ''
      }
    },
    methods: {
      createPost(post){
        this.posts.push(post);
        this.dialogVisible = false;
      },
      deletePost(post){
        this.posts = this.posts.filter(p => p.id !== post.id)
      },
      showDialog(){
        this.dialogVisible = true;
      },
      async fetchUsers(){
      }
    }
  }
</script>

<style>
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  .app{
    padding: 20px;
  }
</style>