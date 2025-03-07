<script setup>
import { Background, Controls, MiniMap, VueFlow, isNode, useVueFlow } from '@braks/vue-flow'
import { ref } from 'vue'
import { initialElements } from './customized-elements.js'

/**
 * useVueFlow provides all event handlers and store properties
 * You can pass the composable an object that has the same properties as the VueFlow component props
 */
const { onPaneReady, onNodeDragStop, onConnect, instance, addEdges } = useVueFlow()

/**
 * Our elements
 */
const elements = ref(initialElements)

/**
 * This is a Vue Flow event-hook which can be listened to from anywhere you call the composable, instead of only on the main component
 *
 * onPaneReady is called when viewpane & nodes have visible dimensions
 */
onPaneReady(({ fitView }) => {
  fitView()
})

onNodeDragStop((e) => console.log('drag stop', e))

/**
 * onConnect is called when a new connection is created.
 * You can add additional properties to your new edge (like a type or label) or block the creation altogether
 */
onConnect((params) => addEdges([params]))

const dark = ref(false)

/**
 * To update node properties you can simply use your elements v-model and mutate the elements directly
 * Changes should always be reflected on the graph reactively, without the need to overwrite the elements
 */
const updatePos = () =>
  elements.value.forEach((el) => {
    console.log(el, elements.value)
    if (isNode(el)) {
      el.position = {
        x: Math.random() * 400,
        y: Math.random() * 400,
      }
    }
  })

/**
 * toObject transforms your current graph data to an easily persist-able object
 */
const logToObject = () => console.log(instance.value?.toObject())

/**
 * Resets the current viewpane transformation (zoom & pan)
 */
const resetTransform = () => instance.value?.setTransform({ x: 0, y: 0, zoom: 1 })

const toggleClass = () => {
  dark.value = !dark.value
  elements.value.forEach((el) => (el.class = dark.value ? 'dark' : 'light'))
}
</script>

<template>
  <VueFlow v-model="elements" class="basicflow" :default-zoom="1.5" :min-zoom="0.2" :max-zoom="4">
    <template #node-card="{ label }">
      <q-card class="my-card text-white" style="background: radial-gradient(circle, #35a2ff 0%, #014a88 100%)">
        <q-card-section>
          <div class="text-h6">Q-Card Node</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          {{ label }}
        </q-card-section>
      </q-card>
    </template>
    <Background pattern-color="#aaa" gap="8" />
    <MiniMap />
    <Controls />

    <div class="controls">
      <button style="background-color: #113285; color: white" @click="resetTransform">reset transform</button>
      <button style="background-color: #6f3381; color: white" @click="updatePos">update positions</button>
      <button
        :style="{
          backgroundColor: dark ? '#FFFFFB' : '#1C1C1C',
          color: dark ? '#1C1C1C' : '#FFFFFB',
        }"
        @click="toggleClass"
      >
        toggle {{ dark ? 'light' : 'dark' }}
      </button>
      <button @click="logToObject">log toObject</button>
    </div>
  </VueFlow>
</template>
