<script setup lang="ts">
import { ref } from 'vue'
import { VueFlow } from '@vue-flow/core'
import { Background } from '@vue-flow/background'
import { initialEdges, initialNodes } from './JS/initial-elements.js'
import ValueNode from './components/ValueNode.vue'
import OperatorNode from './components/OperatorNode.vue'
import ResultNode from './components/ResultNode.vue'

const nodes = ref(initialNodes)
const edges = ref(initialEdges)

</script>

<template >
  <div class="w-screen h-screen">
    <VueFlow class="math-flow" :nodes="nodes" :edges="edges" fit-view-on-init>
      <template #node-value="props">
        <ValueNode :id="props.id" :data="props.data" />
      </template>

      <template #node-operator="props">
        <OperatorNode :id="props.id" :data="props.data" />
      </template>

      <template #node-result="props">
        <ResultNode :id="props.id" />
      </template>

      <Background />
    </VueFlow>
  </div>
</template>

<style>
/* import the necessary styles for Vue Flow to work */
@import '@vue-flow/core/dist/style.css';

/* import the default theme, this is optional but generally recommended */
@import '@vue-flow/core/dist/theme-default.css';
</style>