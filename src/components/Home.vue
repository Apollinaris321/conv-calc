<template>
  <v-container grid-list-xs class="w-100 d-flex flex-column align-center justify-center">
    <v-container class="justify-center align-center ga-5">
        <v-row class="justify-center align-center">
          <InputSizeForm
            :width="image_size.width"
            :height="this.image_size.height"
            :channels="image_size.channels"
            @update="handleUpdateInput"
            class="w-50"
          ></InputSizeForm>
        </v-row>
        <v-row class="justify-center align-center pb-2" no-gutters>
          <div class="d-flex flex-row justify-center align-center ga-2 ">
            <v-btn color="success" @click="calc()">calc</v-btn>
            <v-sheet>
               result: {{ calc_result }}
            </v-sheet>
          </div>
        </v-row>
        <v-row class="justify-center align-center pb-2 pr-2" no-gutters>
          <div class="d-flex flex-row justify-center align-center ga-2 ">
            <v-btn color="success" @click="addConv()">Conv2d</v-btn>
            <v-btn color="purple" @click="addPool()">MaxPool2d</v-btn>
          </div>
        </v-row>
    </v-container>

    <VueDraggable
      ref="el"
      tag="v-layout"
      v-model="list"
      :animation="150"
      ghostClass="ghost"
      handle=".handle"
      class="w-50"
    >
      <div
        v-for="(item, i) in list"
        :key="item.id"
        :item="item"
        class="p-3 d-flex flex-column pb-2"
      >
          <ConvForm
            v-if="item.title=='conv'"
            @remove="handleIdClicked"
            @update="handleUpdate"
            :id="item.id"
            :conv="item"
            >
          </ConvForm>
          <MaxPoolForm
            v-else
            @remove="handleIdClicked"
            @update="handleUpdate"
            :id="item.id"
            :pool="item"
            :kernel="item.kernel"
          ></MaxPoolForm>
        </div>
    </VueDraggable>

  </v-container>
</template>

<script>
import { VueDraggable } from 'vue-draggable-plus'
import { ref } from 'vue'
import ConvForm from './ConvForm.vue';
import MaxPoolForm from './MaxPoolForm.vue';
import Drag from './Drag.vue';
import Drag2 from './Drag2.vue';
import InputSizeForm from './InputSizeForm.vue';

const toggle = ref()
export default {
  components:{
    VueDraggable
  },
  data () {
    return {
      toggle: "conv2d",
      id_counter: 0,
      list: [],
      image_size: {width: 100, height: 100, channels: 0},
      calc_result: 0,
    }
  },
  methods: {
    addConv(){
      this.list.push({id: this.id_counter, title:"conv", channel: 9, kernel: 3, stride: 1, padding: 0})
      this.id_counter += 1
    },
    addPool(){
      this.list.push({id: this.id_counter, title:"pool", kernel: 2, stride: 2, padding: 0})
      this.id_counter += 1
    },
    // (W – F + 2P) / S + 1.
    calc(){
      // Filter
      // Padding
      // W -> Size
      let result = this.image_size.height
      for(const entry of this.list){
        if(entry.title === "conv"){
          result = this.calc_conv(result, entry.kernel, entry.stride, entry.padding)
        }else if(entry.title === "pool"){
          result = this.calc_conv(result, entry.kernel, entry.stride, 0)
        }
      }
      this.calc_result = result
    },
    calc_conv(w, f, s, p){
      return Math.floor((w - f + 2 * p) / s) + 1
    },
    showList(){
      console.log("list: ", ...this.list);
      console.log("image :", this.image_size);
    },
    handleIdClicked(id){
      const index = this.list.findIndex(e => e.id === id)
      this.list.splice(index, 1)
      //this.list = this.list.filter(e => e.id !== id)
    },
    handleUpdate(id, attr, newVal){
      console.log("updating..." , id, ", attr: ", attr);

      let index = this.list.findIndex(e => e.id === id)
      console.log("index: ", index);

      this.list[index][attr] = newVal
    },
    handleUpdateInput(attr, newVal){
      this.image_size[attr] = newVal
    }
  }
}

</script>

<style scoped>
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
</style>

