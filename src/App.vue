<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <div class="app__btns">
      <!--    <input type="text" v-model.number="modificatorValue">-->
      <my-button @click="showDialog">Создать пост</my-button>
      <my-select
          v-model="selectedSort"
          :options="sortOptions"
      />
    </div>
    <my-dialog v-model:show="dialogVisible">
      <PostForm @create="createPost"/>
    </my-dialog>
    <PostList v-if="!isPostsLoading"
        :posts="sortedPosts" @remove="deletePost"/>
    <div v-else>Идет загрузка...</div>
  </div>
</template>

<script>
  import PostForm from "@/components/PostForm";
  import PostList from "@/components/PostList";
  import axios from "axios";
  export default {
    components: { PostList, PostForm},
    data(){
      return{
        posts: [],
        dialogVisible: false,
        modificatorValue: '',
        isPostsLoading: true,
        selectedSort: '',
        sortOptions: [
          {value: 'title', name: 'По названию'},
          {value: 'body', name: 'По описанию'}
        ]
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
      async fetchPosts(){
        try{
            const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
            this.posts = response.data;
        }catch (e) {
          alert('Ошибка')
        } finally {
          this.isPostsLoading = false;
        }
      }
    },
    mounted(){
      this.fetchPosts()
    },
    computed: {
      sortedPosts(){
        return [...this.posts].sort((post1, post2) => {
          return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
        })
      }
    },
    watch: {

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
  .app__btns{
    margin: 15px 0;
    display: flex;
    justify-content: space-between;
  }
</style>