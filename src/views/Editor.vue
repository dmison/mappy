<template>
  <div class=editor-page>
    <button @click="add">+</button>
    <Canvas @endDrag="endDrag" @move="move" @canvas-size="setCanvasSize">
      <Node
        v-for="node in nodes" :key="node.id"
        :id="node.id"
        :x-pos="node.xPos"
        :y-pos="node.yPos"
        :width="node.width"
        :height="node.height"
        @startDrag="startDrag(node)">
          <Note
            :title="node.title"
            :content="node.content"
          />
      </Node>
    </Canvas>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import { v4 as uuidv4 } from 'uuid'
import Canvas from '@/components/Canvas.vue'
import Node from '@/components/Node.vue'
import Note from '@/components/Note.vue'
import { Node as NodeType } from '@/types'

export default defineComponent({
  name: 'Editor',
  components: {
    Canvas,
    Node,
    Note
  },
  setup () {
    const nodes = ref<Array<NodeType>>([{
      id: uuidv4(),
      title: 'untitled',
      content: '',
      xPos: 10,
      yPos: 10,
      width: 180,
      height: 100
    }])

    const dragging = ref(false)
    const draggedNode = ref<NodeType | null>(null)
    const canvasConstraints = ref<{
      height: number;
      left: number;
      top: number;
      width: number;
    } | null>(null)

    function add () {
      nodes.value.push(
        {
          id: uuidv4(),
          title: 'untitled',
          content: '',
          xPos: 10,
          yPos: 10,
          width: 180,
          height: 100
        }
      )
    }

    function setCanvasSize (bounds: {
      height: number;
      left: number;
      top: number;
      width: number;
    }) {
      canvasConstraints.value = bounds
    }

    function startDrag (node: NodeType) {
      if (node) {
        draggedNode.value = node
        dragging.value = true
      }
    }

    function endDrag () {
      draggedNode.value = null
      dragging.value = false
    }

    function move ({
      movementX,
      movementY
    }: { movementX: number; movementY: number }) {
      if (draggedNode.value !== null) {
        draggedNode.value.xPos += movementX
        draggedNode.value.yPos += movementY
      }
      if (draggedNode.value !== null && canvasConstraints.value !== null) {
        if (draggedNode.value.xPos < 0) draggedNode.value.xPos = 0
        if (draggedNode.value.yPos < 0) draggedNode.value.yPos = 0
      }
    }
    return {
      canvasConstraints,
      nodes,
      dragging,
      draggedNode,
      setCanvasSize,
      add,
      startDrag,
      endDrag,
      move
    }
  }
})
</script>
