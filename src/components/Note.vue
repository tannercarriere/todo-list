<template>
    <div class="note-box">
        <h4>Notes on "{{this.task}}"</h4>
        <p v-if="!edit" class="message-box">
            {{this.message}}
        </p>
        <textarea v-else class="message-box" id="edit-note" type="text" v-on:keyup.enter="editSwitch()" :value="this.message"/>
        <input type="button" class="button" value="Add/Edit Note" @click="editSwitch()"/>
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
          this.$emit('edit')
        }
        this.edit = !this.edit;
      }
  }
}
</script>
