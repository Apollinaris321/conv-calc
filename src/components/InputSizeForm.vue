<template>
  <v-container grid-list-xs>
    <v-row>
      <v-text-field
        v-model.number="height"
        label="height"
        id="id"
      ></v-text-field>
      <v-text-field
        v-model.number="width"
        label="width"
        id="id"
      ></v-text-field>
      <v-text-field
        v-model.number="channels"
        label="channels"
        id="id"
      ></v-text-field>
    </v-row>
  </v-container>
</template>

<script setup>

import { ref, watch } from 'vue';

const props = defineProps(['width', 'height', 'channels'])
const emits = defineEmits(["update"])
const width = ref(props.width)
const height = ref(props.height)
const channels =  ref(props.channels)

watch(width, (newVal, old) => emits('update', 'width', newVal))
watch(height, (newVal, old) => emits('update', 'height', newVal))
watch(channels, (newVal, old) => emits('update', 'channels', newVal))

</script>
