<template>
    <div :class="{ 'todo-container-unchecked': done, 'todo-container-checked': !done }">
        <input type="checkbox" class="check-box" v-model="done"><span class="todo-object">{{todo}}</span><br/>
        <input type="button" value="Delete" class="delete-button" v-on:click="rm()">
    </div>
</template>

<script>
export default {
  data () {
    let done= false;
    return {
      done
    }
  },
  props: {
      todo: { required: true, type: String }
  },
  methods: {
      rm(){
        this.$emit('delete');
      }
  }
}
</script>
