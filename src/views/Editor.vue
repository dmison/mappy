<template>
  <div class=editor-page>
    <button @click="add">+</button>
    <Canvas v-model:nodes="nodes" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import { v4 as uuidv4 } from 'uuid'
import Canvas from '@/components/Canvas.vue'
import { Node as NodeType } from '@/types'
export default defineComponent({
  name: 'Editor',
  components: { Canvas },
  setup () {
    const nodes = ref<Array<NodeType>>([{
      id: uuidv4(),
      title: 'untitled',
      content: '',
      xPos: 10,
      yPos: 10
    }])
    function add () {
      console.log('add')
      nodes.value.push(
        {
          id: uuidv4(),
          title: 'untitled',
          content: '',
          xPos: 10,
          yPos: 10
        }
      )
    }
    return {
      nodes,
      add
    }
  }
})
</script>

<style lang="sass" scoped>

</style>
