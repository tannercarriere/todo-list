<template>
  <div id="app">
      <h1 class="title">TO-DO LIST</h1>
      <ul class="list-container" >
        <li>
          <input ref="push" class="input" type="text" @keyup.enter="push()" v-model="todo">
        </li>
        <li v-for="item in todos" :key="item">
          <to-do-item v-show="!isDone" :todo=item v-on:delete="pop(item)"/>
        </li>
      </ul>
      <!--h1 v-if="done.length > 0" class="title">TO-DO LIST</h1>
      <ul class="list-container">
        <li v-for="item in todos" :key="item">
          <to-do-item :todo=item />
        </li>
      </ul-->
      
  </div>
</template>

<script>
import ToDoItem from './components/ToDoItem.vue'
export default {
  name: 'App',
  data () {
    return {
      isDone: false,
      todo: "",
      todos: [],
      done: []
    }
  }, mounted(){
    if(localStorage.todos.length>0){
      this.todos = Array(localStorage.length);
      for(var i = 0; i < localStorage.todos.length; i++){
        this.todos[i] = localStorage.todos[i];
      }
      localStorage.todos = null;
    }
  },
  watch:{
    todos:{
      deep: true,
      handler(newTodos){
        localStorage.todos = Array(newTodos.length);
        for(var i = 0; i < newTodos.length; i++){
          localStorage.todos[i] = newTodos[i];
        }
      }
    }
    
  },
  methods: {
    push(){
      this.todos.push(this.todo);
      this.cur++;
      this.todo = ""
    },
    pop(element){
      for(var i = 0; i < this.todos.length; i++){
        if(element == this.todos[i]){
            this.todos.splice(i, 1); 
            i--; 
        }
      }
    }
  },
  components:{
      ToDoItem
  }
}
</script>

<style>
  *{
    margin:0px;
    font-family: Arial, Helvetica, sans-serif;
  }
  #app{
    padding: 10px;
    background-color: lightslategrey;
    height: 100vh;
  }
  ul{
    padding: 0px;
  }
  li{
    list-style-type: none;
  }
  .title{
    text-align: center;
  }
  .check-box{
    margin: 5px;
    text-align: center;
  }
  .todo-container-unchecked{
    background-color: rgba(255,255,255,.25);
    margin: 5px;
    padding: 10px;
  }
  .todo-container-checked{
    background-color: rgba(255,255,255,.5);
    margin: 5px;
    padding: 10px;
  }
  .list-container {
    width: 90%;
    margin: auto;
  }
  .input{
    width: 100%;
    height: 40px;
    font-size: 20px;
    background-color: rgba(255,255,255,.5);
  }
  .input:focus{
    background-color: rgba(255, 255, 255, 0.75);
  }
  .delete-button{
    background-color: lightslategray;
    border: none;
  }
</style>
