<script setup>
import { Controls, MiniMap, VueFlow, useVueFlow } from '@braks/vue-flow'
import { ref, watchEffect } from 'vue'

const isHidden = ref(false)

const { nodes, edges } = useVueFlow({
  nodes: [
    { id: '1', type: 'input', label: 'Node 1', position: { x: 250, y: 5 } },
    { id: '2', label: 'Node 2', position: { x: 100, y: 100 } },
    { id: '3', label: 'Node 3', position: { x: 400, y: 100 } },
    { id: '4', label: 'Node 4', position: { x: 400, y: 200 } },
  ],
  edges: [
    { id: 'e1-2', source: '1', target: '2' },
    { id: 'e1-3', source: '1', target: '3' },
    { id: 'e3-4', source: '3', target: '4' },
  ],
})

watchEffect(() => {
  nodes.value.forEach((n) => (n.hidden = isHidden.value))
  edges.value.forEach((e) => (e.hidden = isHidden.value))
})
</script>

<template>
  <VueFlow>
    <MiniMap />
    <Controls />

    <div :style="{ position: 'absolute', left: '10px', top: '10px', zIndex: 4 }">
      <div>
        <label for="ishidden">
          hidden
          <input id="ishidden" v-model="isHidden" type="checkbox" />
        </label>
      </div>
    </div>
  </VueFlow>
</template>
