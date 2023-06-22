<template>
  <div class="app">
      <form>
        <h4>Создание поста</h4>
        <input v-bind:value="title"
               @input="titleValue"
               class="input"
               type="text"
               placeholder="Название поста">
        <input v-bind:value="body" class="input" type="text" placeholder="Описание поста">
        <button class="btn" type="submit" @click="createPost">Создать</button>
      </form>
    <div class="post" v-for="post in posts">
      <div><strong>Название:</strong> {{post.title}}</div>
      <div><strong>Описание:</strong> {{post.body}}</div>
    </div>
  </div>
</template>

<script>
  export default {
    data(){
      return{
        posts: [
          {id: 1, title: 'Javascript 1', body: 'Описание поста 1'},
          {id: 2, title: 'Javascript 2', body: 'Описание поста 2'},
          {id: 3, title: 'Javascript 3', body: 'Описание поста 3'},
          {id: 4, title: 'Javascript 4', body: 'Описание поста 4'},
        ],
        title: '',
        body: '',
      }
    },
    methods: {
      createPost(){
        console.log()
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
  form{
    display: flex;
    flex-direction: column;
  }
  .post{
    padding: 15px;
    border: 2px solid teal;
    margin-top: 15px;
  }
  .input{
    width: 100%;
    border: 1px solid teal;
    padding: 10px 15px;
    margin-top: 15px;
  }
  .btn{
    align-self: flex-end;
    margin-top: 15px;
    padding: 10px 15px;
    background: none;
    color: teal;
    border: 1px solid teal;
    cursor: pointer;
  }
</style>