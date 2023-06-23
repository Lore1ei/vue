<template>
  <div>
    <h1>Страница с постами</h1>
    <my-input
        v-model="searchQuery"
        placeholder="Поиск..."
    />
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
              :posts="sortedAndSearchedPosts" @remove="deletePost"/>
    <div v-else>Идет загрузка...</div>
    <div ref="observer" class="observer"></div>
    <!--    <div class="page_wrapper">-->
    <!--      <div class="page"-->
    <!--           v-for="pageNumber in totalPages"-->
    <!--           :key="page"-->
    <!--           :class="{'current-page': page === pageNumber}"-->
    <!--           @click="changePage(pageNumber)"-->
    <!--      >{{pageNumber}}</div>-->
    <!--    </div>-->
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import axios from "axios";
import MyInput from "@/components/UI/MyInput";
export default {
  components: {MyInput, PostList, PostForm},
  data(){
    return{
      posts: [],
      dialogVisible: false,
      modificatorValue: '',
      page: 1,
      limit: 10,
      totalPages: 0,
      isPostsLoading: true,
      selectedSort: '',
      searchQuery: '',
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
    // changePage(pageNumber){
    //     this.page = pageNumber
    // },
    async fetchPosts(){
      try{
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data;
      }catch (e) {
        alert('Ошибка')
      } finally {
        this.isPostsLoading = false;
      }
    },
    async loadMorePosts(){
      try{
        this.page +=1;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data]
      }catch (e) {
        alert('Ошибка')
      } finally {
        this.isPostsLoading = false;
      }
    },
  },
  mounted(){
    this.fetchPosts()
    let options = {
      rootMargin: "0px",
      threshold: 1.0,
    };
    const callback = (entries, observer) => {
      if(entries[0].isIntersecting && this.page < this.totalPages){
        this.loadMorePosts();
      }
    }

    let observer = new IntersectionObserver(callback, options);
    observer.observe(this.$refs.observer)
  },
  computed: {
    sortedPosts(){
      return [...this.posts].sort((post1, post2) => {
        return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
      })
    },
    sortedAndSearchedPosts(){
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },
  watch: {
    // page(){
    //   this.fetchPosts()
    // }
  }
}
</script>

<style>
.page_wrapper{
  display: flex;
  margin-top: 15px;
  gap: 4px;
}
.page{
  border: 1px solid black;
  padding: 6px;
  cursor: pointer;
}
.current-page{
  border: 2px solid teal;
}

</style>