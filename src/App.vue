<template>
  <div id="app">
      <h1 class="title">TO-DO LIST</h1>
      <ul class="list-container" >
        <li>
          <input class="input" type="text" @keyup.enter="push()" v-model="todo">
        </li>
        <li v-for="(item, index) in todos" :key="item.id">
          <to-do-item :todo="item.task" :id="index" @delete="pop(index)" @edit="edit(index)" @notes="showNote(index)"/>
        </li>
        <div ref="notes"></div>
      </ul>
  </div>
</template>

<script>
import ToDoItem from './components/ToDoItem.vue'
import Note from './components/Note.vue'
import Vue from 'vue'
export default {
  name: 'App',
  data () {
    return {
      todo: "",
      todos: [],
      notes: []
    }
  }, mounted(){
      if(localStorage.getItem("todos")){//if we have values in local storage set those as default
        this.todos = JSON.parse(localStorage.getItem("todos"));
      }
      if(localStorage.getItem("notes")){//if we have values in local storage set those as default
        this.notes = JSON.parse(localStorage.getItem("notes"));
      }
  },
  watch:{
    todos:{
      deep: true,
      handler(){//if the todos array changes store the change
        localStorage.setItem('todos', JSON.stringify(this.todos));
      }
    },
    notes:{
      deep: true,
      handler(){
        console.log(this.notes)
        localStorage.setItem('notes', JSON.stringify(this.notes));
      }
    }
    
  },
  methods: {
    push(){//push an new object to the list 
      this.todos.push(
        {
          task:this.todo, 
          id:Date.now()
        }
      ); //the id allows us to have tasks of duplicate names
      this.notes.push(
        {
          task:this.todo, 
          message:""
        }
      );
      this.todo = "";
    },
    pop(index){
      this.todos.splice(index, 1);//drop the item at the given index
      this.notes.splice(index, 1);
    },
    edit(index){
      this.todos[index].task = document.getElementById("edit").value; // change the task at the given index
    },
    showNote(index){
      console.log("lmao")
      let ComponentClass = Vue.extend(Note);
      let instance = new ComponentClass({
        propsData: {
          task: this.notes[index].task,
          message: this.notes[index].message
        }
      });
      instance.$mount();
      this.$refs.notes.appendChild(instance.$el);
      this.$on.edit('edit', function(){

      });
    }
  },
  components:{
      ToDoItem,
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
  .todo-container{
    margin: 1rem 1.5rem;
    padding: 10px;
  }
  .checked{
    background-color: rgba(255,255,255,.5);
  }
  .unchecked{
    background-color: rgba(255,255,255,.25);
    color: rgba(255,255,255,.5);
  }
  .list-container {
    display: inline-block;
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
    margin-top: .25rem;
    margin-right: .1rem;
  }
  .note-box{
    background-color: rgba(255,255,255,.5);
    width: 24rem;
    height: 25rem;
    padding: 1rem;
  }
  .message-box{
    background-color: rgba(255,255,255,.5);
    width: 95%;
    height: 90%;
  }
</style>
