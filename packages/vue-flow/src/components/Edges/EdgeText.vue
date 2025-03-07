<script lang="ts" setup>
import type { EdgeTextProps } from '../../types/components'
import type { Rect } from '../../types'

const {
  x,
  y,
  label,
  labelStyle = {},
  labelShowBg = true,
  labelBgStyle = {},
  labelBgPadding = [2, 4],
  labelBgBorderRadius = 2,
} = defineProps<EdgeTextProps>()

const edgeRef = templateRef<SVGTextElement>('el', null)

let box = $ref<Rect>({ x: 0, y: 0, width: 0, height: 0 })

onMounted(() => {
  box = edgeRef.value.getBBox()
})

const transform = computed(() => `translate(${x - box.width / 2} ${y - box.height / 2})`)
</script>

<script lang="ts">
export default {
  name: 'EdgeText',
}
</script>

<template>
  <g :transform="transform" class="vue-flow__edge-textwrapper">
    <rect
      v-if="labelShowBg"
      class="vue-flow__edge-textbg"
      :width="`${box.width + 2 * labelBgPadding[0]}px`"
      :height="`${box.height + 2 * labelBgPadding[1]}px`"
      :x="-labelBgPadding[0]"
      :y="-labelBgPadding[1]"
      :style="labelBgStyle"
      :rx="labelBgBorderRadius"
      :ry="labelBgBorderRadius"
    />
    <text v-bind="$attrs" ref="el" class="vue-flow__edge-text" :y="box.height / 2" dy="0.3em" :style="labelStyle">
      <slot>
        <component :is="label" v-if="typeof label !== 'string' && typeof label" />
        <template v-else>
          {{ label }}
        </template>
      </slot>
    </text>
  </g>
</template>
