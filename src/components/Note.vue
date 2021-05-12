<template>
    <div class="note-box">
      
      <h4 class="note-title">Notes on "{{this.task}}"</h4>
      <a class="close" @click="close()">&times;</a>
      <p v-if="!edit" class="message-box">
          {{this.message}}
      </p>
      <textarea v-else class="message-box" id="edit-note" type="text" v-on:keyup.enter="editSwitch()" :value="this.message"/>
      <input v-if="!edit" type="button" class="button" value="Add/Edit Note" @click="editSwitch()"/>
      <input v-else type="button" class="button" value="Submit Note" @click="editSwitch()"/>
    </div>
</template>

<script>
export default {
  data () {
    let edit= false;
    return {
      edit
    }
  },
  props: {
    task: {required: true, type: String},
    message: {required: true, type: String}
  },
  methods: {
    editSwitch(){
        if(this.edit){//this will only emit if in edit mode
          this.$emit('edit');
        }
        this.edit = !this.edit;
    },
    close(){
      this.$emit('close');
    }
  }
}
</script>
