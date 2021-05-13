<template>
    <div v-bind:class="{ 'todo-container unchecked': !done, 'todo-container checked': done }" >
      <!-- Conditionally render the checkbox if we're not editing -->
      <input v-if="!edit" type="checkbox" class="check-box" v-model="done">
        <!-- if not editing render the task as plain text-->
        <span v-if="!edit" class="todo-object">{{todo}}</span>
        <!-- if  editing render the task as an input field with the value set to it's previous text-->
        <span v-else><input id="edit" type="text" v-on:keyup.enter="editSwitch()" :value="todo"></span><br/>

      <input type="button" value="Delete" class="button" v-on:click="rm()">&nbsp;
      <span v-if="!edit">
        <input  type="button" value="Edit" class="button" v-on:click="editSwitch()">
      </span>
      <span v-else>
        <input type="button" value="Submit" class="button" v-on:click="editSwitch()" >
        <input v-if="!pickingColor" class="button" type="button" value="Change color" @click="colorSwitch()"/>
        <ul v-else class="color-picker">
          <li class="color" ><input id="red" type="button" @click="sendColor('red')"></li>
          <li class="color" ><input id="yellow" type="button" @click="sendColor('yellow')"></li>
          <li class="color" ><input id="none" type="button" @click="sendColor('none')"></li>
        </ul>
      </span>
      <input type="button" value="Note" class="button" v-on:click="noteRequest()" >
      <slot></slot>
    </div>
</template>

<script>
export default {
  data () {
    let done= false;
    let edit= false;
    let pickingColor = false;
    return {
      done,
      edit,
      pickingColor
    }
  },
  props: {
      todo: { required: true, type: String }
  },
  methods: {
      rm(){
        this.$emit('delete');
      },
      editSwitch(){
        if(this.edit){//this will only emit if in edit mode
          this.$emit('edit')
        }
        this.edit = !this.edit;
      },
      noteRequest(){
        this.$emit('notes');
      },
      colorSwitch(){
        this.pickingColor = !this.pickingColor
      },
      sendColor(color){
        this.$emit("color-change", color);
      }
  }
}
</script>
