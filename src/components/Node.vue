<template>
  <div  @mousedown="startDrag"
        :style="{ top: `${yPos}px`, left: `${xPos}px`, width: `${width}px`, height: `${height}px`}">
    <slot name="default" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Node',
  props: {
    id: { type: String, required: true },
    xPos: { type: Number, required: true },
    yPos: { type: Number, required: true },
    height: { type: Number, required: true },
    width: { type: Number, required: true }
  },
  setup (props, { emit }) {
    function startDrag () {
      emit('startDrag')
    }
    return {
      startDrag
    }
  }
})
</script>

<style lang="sass" scoped>
div
  position: absolute
  overflow-x: scroll
</style>
