<script lang="ts" setup>
import type { Connection, Edge, Elements } from '@braks/vue-flow'
import { Background, Controls, MiniMap, VueFlow, addEdge } from '@braks/vue-flow'
import { ref } from 'vue'
import { ResizeRotateNode } from '../src'
import initialElements from './elements'

const elements = ref<Elements>(initialElements)
const onConnect = (params: Edge | Connection) => (elements.value = addEdge(params, elements.value))
</script>

<template>
  <div style="height: 100%">
    <VueFlow v-model="elements" class="vue-flow-basic-example" :fit-view-on-init="true" @connect="onConnect">
      <template #node-resize-rotate="props">
        <ResizeRotateNode v-bind="props" />
      </template>
      <Controls />
      <MiniMap />
      <Background color="#aaa" :gap="8" />
    </VueFlow>
  </div>
</template>

<style>
@import '@braks/vue-flow/dist/style.css';
@import '@braks/vue-flow/dist/theme-default.css';

html,
body,
#root {
  height: 100%;
  width: 100%;
  font-family: 'JetBrains Mono', monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-transform: uppercase;
  color: white;
}
</style>
