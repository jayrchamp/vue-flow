<script setup>
import { EdgeText, getBezierPath, getEdgeCenter } from '@braks/vue-flow'
import { computed } from 'vue'

const props = defineProps({
  id: {
    type: String,
    required: true,
  },
  sourceX: {
    type: Number,
    required: true,
  },
  sourceY: {
    type: Number,
    required: true,
  },
  targetX: {
    type: Number,
    required: true,
  },
  targetY: {
    type: Number,
    required: true,
  },
  sourcePosition: {
    type: String,
    required: true,
  },
  targetPosition: {
    type: String,
    required: true,
  },
  data: {
    type: Object,
    required: false,
  },
  markerEnd: {
    type: String,
    required: false,
  },
  style: {
    type: Object,
    required: false,
  },
  sourceHandleId: {
    type: String,
    required: false,
  },
  targetHandleId: {
    type: String,
    required: false,
  },
})

const edgePath = computed(() =>
  getBezierPath({
    sourceX: props.sourceX,
    sourceY: props.sourceY,
    sourcePosition: props.sourcePosition,
    targetX: props.targetX,
    targetY: props.targetY,
    targetPosition: props.targetPosition,
  }),
)

const center = computed(() =>
  getEdgeCenter({
    sourceX: props.sourceX,
    sourceY: props.sourceY,
    targetX: props.targetX,
    targetY: props.targetY,
  }),
)
const onClick = () => console.log(props.data)
</script>

<script>
export default {
  inheritAttrs: false,
}
</script>

<template>
  <path :id="props.id" class="vue-flow__edge-path" :d="edgePath" :marker-end="props.markerEnd" />
  <EdgeText
    :x="center[0]"
    :y="center[1]"
    :label="props.data?.text"
    :label-style="{ fill: 'white' }"
    :label-show-bg="true"
    :label-bg-style="{ fill: '#10b981' }"
    :label-bg-padding="[2, 4]"
    :label-bg-border-radius="2"
    @click="onClick"
  />
</template>
