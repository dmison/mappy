<template>
  <div ref="root"
       class="canvas"
       @mouseup="endDrag"
       @mousemove="move" >
    <slot name="default" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue'

export default defineComponent({
  name: 'Canvas',
  setup: function (props, { emit }) {
    const root = ref<HTMLDivElement|null>(null)

    onMounted(() => {
      emit('canvas-size', {
        height: root.value?.offsetHeight,
        left: root.value?.offsetLeft,
        top: root.value?.offsetTop,
        width: root.value?.offsetWidth
      })
    })
    function endDrag () {
      emit('endDrag')
    }
    function move ({ movementX, movementY }: { movementX: number; movementY: number }) {
      emit('move', { movementX, movementY })
    }
    return {
      move,
      endDrag,
      root
    }
  }
})
</script>

<style lang="sass" scoped>
.canvas
  height: 80vh
  width: 100vw
  border: 1px solid grey
  background-color: #eee
  position: relative
  user-select: none
</style>
