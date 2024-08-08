<template>
  <v-sheet class="mx-auto">
    <v-container grid-list-xs>
      <v-card-title primary-title>
        Conv2d {{ props.id }}
      </v-card-title>
      <v-row>
        <v-text-field
          v-model.number="out_channel"
          :rules="numberRules"
          label="channel"
        ></v-text-field>

        <v-text-field
          v-model.number="kernel"
          :rules="numberRules"
          label="kernel"
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
        <v-btn color="red" @click="remove()">delete</v-btn>
      </v-row>
    </v-container>
  </v-sheet>
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
