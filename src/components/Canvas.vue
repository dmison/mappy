<template>
  <div class="canvas" ref="root" @mouseup="endDrag" @mousemove="move" >
    <Node
      v-for="node in nodes" :key="node.id"
      :id="node.id"
      :title="node.title"
      :content="node.content"
      :x-pos="node.xPos"
      :y-pos="node.yPos"
      @startDrag="startDrag"

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
  setup: function (props) {
    const dragging = ref(false)
    const draggedNode = ref<NodeType | null>(null)
    const root = ref(null)

    function startDrag (id: string) {
      const node = props.nodes.find(node => node.id === id)

      if (node) {
        draggedNode.value = node
        dragging.value = true
      }
    }

    onMounted(() => {
      console.log(root)
      // offsetHeight: 434// offsetLeft: 8// offsetParent: body// offsetTop: 112// offsetWidth: 1624
    })
    onUpdated(() => {
      // console.log(root)
    })

    function endDrag () {
      draggedNode.value = null
      dragging.value = false
    }

    function move ({ movementX, movementY }: { movementX: number; movementY: number }) {
      if (draggedNode.value !== null) {
        draggedNode.value.xPos += movementX
        draggedNode.value.yPos += movementY
      }
    }

    return {
      dragging,
      draggedNode,
      move,
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
