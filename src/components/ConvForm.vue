<template>
  <v-card class="pa-3 d-flex flex-row justify-center align-center rounded-xl ga-2">
    <v-icon class="handle cursor-grabbing" icon="mdi-drag-horizontal" size="x-large"></v-icon>
    <div class="d-flex flex-row justify-space-between">
      <v-card-title primary-title class="justify-center align-center text-center">
        Conv2d
      </v-card-title>
    </div>

    <v-text-field
      v-model.number="out_channel"
      :rules="numberRules"
      label="channel"
      color="primary"
    ></v-text-field>

    <v-text-field
      v-model.number="kernel"
      :rules="numberRules"
      label="kernel"
      color="primary"
    ></v-text-field>

    <v-text-field
      v-model.number="stride"
      :rules="numberRules"
      label="stride"
      color="primary"
    ></v-text-field>

    <v-text-field
      v-model.number="padding"
      :rules="numberRules"
      label="padding"
      color="primary"
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
const props = defineProps(['id', 'conv'])

const conv = ref({...props.conv})

const out_channel = ref(props.conv.channel)
const kernel = ref(props.conv.kernel)
const stride = ref(props.conv.stride)
const padding = ref(props.conv.padding)

const numberRules = [
        value => {
          if (value || value === 0) return true

          return 'Can\'t be empty!.'
        },
        value => {
          if (/[0-9]/.test(value)) return true
          return 'Only Integers allowed!'
        },
      ]

function remove(){
  emits('remove', props.id);
}

watch(out_channel, (newVal, old) => emits('update', props.id, 'channel', newVal))
watch(kernel, (newVal, old) => emits('update', props.id, 'kernel', newVal))
watch(stride, (newVal, old) => emits('update', props.id, 'stride', newVal))
watch(padding, (newVal, old) => emits('update', props.id, 'padding', newVal))
</script>
