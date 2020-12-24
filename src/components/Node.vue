<template>
  <div class="node"
       @mousemove="move"
       @mousedown="startDrag"
       @mouseup="endDrag"
       :style="{ top: `${yPos-120}px`, left: `${xPos-20}px`}">
    <h1>{{title}}</h1>
    <p>{{content}}</p>
    {{xPos}}, {{yPos}}
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Node',
  props: {
    id: { type: String, required: true },
    title: { type: String, required: true },
    content: { type: String, required: true },
    xPos: { type: Number, required: true },
    yPos: { type: Number, required: true }
  },
  setup (props, { emit }) {
    function startDrag () {
      emit('startDrag', props.id)
    }
    function endDrag () {
      emit('endDrag', props.id)
    }
    function move (event: any) {
      emit('move', props.id, event.movementX, event.movementY)
    }
    return {
      move,
      startDrag,
      endDrag
    }
  }
})
</script>

<style lang="sass" scoped>
h1
  font-size: 18px
  font-weight: bold
  background-color: grey
  margin: 0
  padding: 2px
.node
  margin: 0
  border: 1px solid black
  position: absolute
  width: 70px
  height: 100px
</style>
