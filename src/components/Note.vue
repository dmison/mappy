<template>
  <div class="note" @dblclick="editStart">
    <template v-if="editing">
      <input value="title" />
      <textarea :value="title" />
      <button @click="editDone">Done</button>
    </template>
    <template v-else>
      <h1>{{title}}</h1>
      <p>{{content}}</p>
    </template>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  props: {
    title: { type: String, required: true },
    content: { type: String, required: true }
  },
  setup () {
    const editing = ref(false)
    function editStart () {
      editing.value = true
    }
    function editDone () {
      editing.value = false
    }

    return {
      editing,
      editStart,
      editDone
    }
  }
})

</script>

<style lang="sass" scoped>
.note
  margin: 0
  border: 1px solid black
  width: 100%
  height: 100%
  background-color: ghostwhite

h1
  font-size: 18px
  font-weight: bold
  background-color: grey
  margin: 0
  padding: 2px
</style>
