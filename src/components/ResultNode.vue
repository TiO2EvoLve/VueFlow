<script setup>
import { computed } from 'vue'
import { Handle, Position, useNodeConnections, useNodesData } from '@vue-flow/core'

defineProps(['id'])

const mathFunctions = {
  '+': (a, b) => a + b,
  '-': (a, b) => a - b,
  '*': (a, b) => a * b,
  '/': (a, b) => a / b,
}

// 获取结果节点的源连接。在这个例子中，只有一个算符节点.
const sourceConnections = useNodeConnections({
  // 类型目标指所有该节点为目标的连接
  // 这意味着我们要在图中倒推寻找连接的来源
  handleType: 'target',
})

// 获取节点的源连接
const operatorSourceConnections = useNodeConnections({
  handleType: 'target',
  nodeId: () => sourceConnections.value[0]?.source,
})

const operatorData = useNodesData(() => sourceConnections.value.map((connection) => connection.source))

const valueData = useNodesData(() => operatorSourceConnections.value.map((connection) => connection.source))

const result = computed(() => {
  const currResult = operatorData.value.reduce((acc, { data }) => {
    const operator = data?.operator

    if (operator) {
      const [a, b] = valueData.value.map(({ data }) => data?.value)

      if (a && b) {
        return mathFunctions[operator](a, b)
      }
    }

    return acc
  }, 0)

  // 四舍五入到小数点后2位
  return Math.round(currResult * 100) / 100
})
</script>

<template>
  <div class="calculation">
    <template v-for="(value, i) in valueData" :key="`${value.id}-${value.data}`">
      <span>
        {{ value.data?.value }}
      </span>

      <span v-if="i !== valueData.length - 1">
        {{ operatorData[0].data?.operator }}
      </span>
    </template>
  </div>

  <span> = </span>

  <span class="result" :style="{ color: result > 0 ? '#5EC697' : '#f15a16' }">
    {{ result }}
  </span>

  <Handle
      type="target"
      :position="Position.Left"
      :connectable="false"
      :style="{ background: result > 0 ? '#5EC697' : '#f15a16' }"
  />
</template>
