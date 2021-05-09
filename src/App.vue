<template>
  <div id="app">
      <h1 class="title">TO-DO LIST</h1>
      <ul class="list-container" >
        <li>
          <input ref="push" class="input" type="text" @keyup.enter="push()" v-model="todo">
        </li>
        <li v-for="(item, index) in todos" :key="item.id">
          <to-do-item v-show="!isDone" :todo="item.task" :id="index" v-on:delete="pop(index)" v-on:input="edit(index)"/>
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
      todos: Array(),
      done: []
    }
  }, mounted(){
      if(localStorage.getItem("todos")){
        this.todos = JSON.parse(localStorage.getItem("todos"));
      }
  },
  watch:{
    todos:{
      deep: true,
      handler(){
        localStorage.setItem('todos', JSON.stringify(this.todos));
      }
    }
    
  },
  methods: {
    push(){
      this.todos.push({task:this.todo, id:Date.now()});
      this.todo = ""
    },
    pop(index){
      this.todos.splice(index, 1);
    },
    edit(index){
      this.todos[index].task = document.getElementById("edit").value;
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
  .button{
    background-color: lightslategray;
    border: none;
  }
</style>
