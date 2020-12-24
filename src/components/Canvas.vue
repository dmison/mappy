<template>
  <div class="canvas" ref="root">
    <Node
      v-for="node in nodes" :key="node.id"
      :id="node.id"
      :title="node.title"
      :content="node.content"
      :x-pos="node.xPos"
      :y-pos="node.yPos"
      @startDrag="startDrag"
      @endDrag="endDrag"
      @move="moveIfDragging"
    />

  </div>
</template>

<script lang="ts">
import { defineComponent, ref, PropType, onMounted, onUpdated } from 'vue'
import Node from './Node.vue'
import { Node as NodeType } from '@/types'

export default defineComponent({
  name: 'Canvas',
  components: { Node },
  props: {
    nodes: {
      type: Array as PropType<Array<NodeType>>,
      required: true
    }
  },
  setup (props) {
    const dragging = ref(false)
    const draggedNodeId = ref('')
    const root = ref(null)
    function startDrag (id: string) {
      draggedNodeId.value = id
      dragging.value = true
    }
    onMounted(() => {
      console.log(root)
      //       offsetHeight: 434
      // offsetLeft: 8
      // offsetParent: body
      // offsetTop: 112
      // offsetWidth: 1624
    })
    onUpdated(() => {
      console.log(root)
    })

    function endDrag () {
      draggedNodeId.value = ''
      dragging.value = false
    }
    function moveIfDragging (id: string, moveX: number, moveY: number) {
      if (dragging.value) {
        const draggedNode = props.nodes.find((node): boolean => node.id === draggedNodeId.value)
        if (draggedNode) {
          draggedNode.xPos += moveX
          draggedNode.yPos += moveY
        }
      }
    }
    return {
      dragging,
      moveIfDragging,
      startDrag,
      endDrag,
      root
    }
  }
})
</script>

<style lang="sass" scoped>
.canvas
  height: 100vh
  width: 100vw
  border: 1px solid grey
  background-color: #eee
  position: relative
</style>
