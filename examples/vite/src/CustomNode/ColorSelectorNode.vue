<script lang="ts" setup>
import type { CSSProperties } from 'vue'
import type { Connection, Edge, NodeProps } from '@braks/vue-flow'
import { Handle, Position } from '@braks/vue-flow'

interface ColorSelectorNodeProps extends NodeProps {
  data: {
    color: string
    onChange: (event: any) => void
  }
}
const props = defineProps<ColorSelectorNodeProps>()

const targetHandleStyle: CSSProperties = { background: '#555' }
const sourceHandleStyleA: CSSProperties = { ...targetHandleStyle, top: '10px' }
const sourceHandleStyleB: CSSProperties = { ...targetHandleStyle, bottom: '10px', top: 'auto' }

const onConnect = (params: Connection | Edge) => console.log('handle onConnect', params)
</script>

<script lang="ts">
export default {
  inheritAttrs: false,
}
</script>

<template>
  <Handle type="target" :position="Position.Left" :style="targetHandleStyle" :on-connect="onConnect" />
  <div>
    Custom Color Picker Node: <strong>{{ data.color }}</strong>
  </div>
  <input class="nodrag" type="color" :value="data.color" @input="props.data.onChange" />
  <Handle id="a" type="source" :position="Position.Right" :style="sourceHandleStyleA" />
  <Handle id="b" type="source" :position="Position.Right" :style="sourceHandleStyleB" />
</template>
