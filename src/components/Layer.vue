<template>
  <v-card class="pa-3 d-flex flex-row justify-center align-center rounded-xl ga-2">
    <div>
      <v-icon class="handle cursor-grabbing" icon="mdi-drag-horizontal" size="x-large"></v-icon>
    </div>
    <div class="d-flex flex-row justify-space-between">
      <v-card-title primary-title class="justify-center align-center text-center">
        MaxPool
      </v-card-title>
    </div>
    <v-text-field
      v-model.number="kernel"
      :rules="numberRules"
      label="kernel"
      class="ma-0"
    ></v-text-field>

    <v-text-field
      v-model.number="stride"
      :rules="numberRules"
      label="stride"
    ></v-text-field>

    <v-text-field
      v-model.number="padding"
      :rules="numberRules"
      label="padding"
    ></v-text-field>
    <div class="d-flex flex-row justify-center align-center">
      <v-btn color="red rounded-m pa-2" min-width="0"  @click="remove">
        <v-icon icon="mdi-close" size="large"></v-icon>
      </v-btn>
    </div>
  </v-card>
</template>

<script setup>
import { stringLiteral } from '@babel/types';
import { ref, watch } from 'vue';

const emits = defineEmits(["remove", "update"])
const props = defineProps(['id', 'kernel', 'pool'])

const kernel = ref(props.pool.kernel)
const stride =  ref(props.pool.stride)
const padding =  ref(props.pool.padding)

/*
const conv = ref({...props.conv})

const out_channel = ref(props.conv.channel)
const kernel = ref(props.conv.kernel)
const stride = ref(props.conv.stride)
const padding = ref(props.conv.padding)
*/

const numberRules = [
        value => {
          if (value) return true
          return 'Can\'t be empty!.'
        },
        value => {
          if (/[0-9]/.test(value)) return true
          return 'Only Integers allowed!'
        },
      ]

function update(attr) {
  emits('update', props.id, attr, kernel.value);
}

function remove(){
  emits('remove', props.id);
}

//TODO
// es wäre besser das ganze objekt einfach zu schicken... aber ich lass das so mal vorerst
watch(kernel, (newVal, old) => emits('update', props.id, 'kernel', newVal))
watch(stride, (newVal, old) => emits('update', props.id, 'stride', newVal))
watch(padding, (newVal, old) => emits('update', props.id, 'padding', newVal))
</script>
