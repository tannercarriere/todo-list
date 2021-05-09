<template>
    <div :class="{ 'todo-container-unchecked': done, 'todo-container-checked': !done }">
      <input v-if="!edit" type="checkbox" class="check-box" v-model="done">
        <span v-if="!edit" class="todo-object">{{todo}}</span>
        <span v-else><input id="edit" type="text" :value="todo"></span><br/>
      <input type="button" value="Delete" class="button" v-on:click="rm()">&nbsp;
      <input v-if="!edit" type="button" value="Edit" class="button" v-on:click="editSwitch()">
      <input v-else type="button" value="Submit" class="button" v-on:click="editSwitch()">
    </div>
</template>

<script>
export default {
  data () {
    let done= false;
    let edit= false;
    return {
      done,
      edit
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
        if(this.edit){
          this.$emit('input')
        }
        this.edit = !this.edit;
      }
  }
}
</script>
