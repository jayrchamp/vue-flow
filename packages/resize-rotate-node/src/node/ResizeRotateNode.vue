<script lang="ts" setup>
import type { MoveableEvents } from 'vue3-moveable'
import Moveable from 'vue3-moveable'
import type { Position } from '@braks/vue-flow'
import { Handle, useVueFlow } from '@braks/vue-flow'

const props = defineProps<{
  id: string
  label: string
  targetPosition: Position
  sourcePosition: Position
  nodeElement: HTMLDivElement
  data: any
}>()

const { viewport, updateNodeDimensions, onPaneClick, onPaneScroll, onPaneContextMenu, getNode, onNodeClick, onNodeDragStart } =
  useVueFlow()

const el = ref()
const visible = ref(false)

const frame = {
  rotate: 0,
}

const onResize = (e: MoveableEvents['resize']) => {
  e.target.style.width = `${e.width}px`
  e.target.style.height = `${e.height}px`
  e.target.style.left = `${e.drag.left}px`
  e.target.style.top = `${e.drag.top}px`
}

const onRotateStart = (e: MoveableEvents['rotateStart']) => {
  e.set(frame.rotate)
}

const onRotate = (e: MoveableEvents['rotate']) => {
  frame.rotate = e.beforeRotate
  e.target.style.transform = `rotate(${e.beforeRotate}deg)`
  updateNodeDimensions([{ id: props.id, nodeElement: props.nodeElement, forceUpdate: true }])
}

const onClick = () => {
  visible.value = true
}

onNodeClick(({ node }) => {
  if (node.id === props.id) onClick()
})

onNodeDragStart(({ node }) => {
  if (node.id === props.id) onClick()
})

const hideMoveable = () => {
  visible.value = false
}

onPaneClick(hideMoveable)
onPaneScroll(hideMoveable)
onPaneContextMenu(hideMoveable)
</script>

<script lang="ts">
export default {
  inheritAttrs: false,
}
</script>

<template>
  <div
    ref="el"
    :data-moveable-id="props.id"
    :style="{
      cursor: 'grab',
      ...data.style,
    }"
    style="position: relative"
    @click="onClick"
  >
    <Handle type="target" :position="props.targetPosition" />
    <slot>
      {{ props.label }}
    </slot>
    <Handle type="source" :position="props.sourcePosition" />
  </div>
  <Moveable
    v-if="visible"
    class-name="nodrag"
    :target="[`[data-moveable-id='${props.id}']`]"
    :resizable="true"
    :rotatable="true"
    rotation-position="top"
    :padding="{ left: 0, top: 0, right: 0, bottom: 0 }"
    :origin="false"
    :edge="true"
    :throttle-resize="1"
    :render-directions="['nw', 'n', 'ne', 'w', 'e', 'sw', 's', 'se']"
    :zoom="0.75"
    @resize="onResize"
    @rotate="onRotate"
  />
</template>
