<template>
  <VueDraggable
    ref="el"
    tag="v-layout"
    v-model="props.liste"
    :animation="150"
    ghostClass="ghost"
  >
    <v-list-item
        v-for="(item, i) in liste"
        :key="i"
        :value="item"
        color="blue"
        background-color="blue"
        class="cursor-move h-30 bg-gray-500/5 rounded p-3"
      >
        <div>{{ item }}</div>
    </v-list-item>
  </VueDraggable>
</template>

<script setup lang="ts">
  import { VueDraggable } from 'vue-draggable-plus'

  const props = defineProps(['liste'])
</script>

<style scoped>
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
</style>
