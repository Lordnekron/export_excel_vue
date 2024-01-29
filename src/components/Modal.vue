<template>
  <div>
    <a-button type="primary" @click="showModal">Open Modal</a-button>
    <a-modal v-model:open="open" title="Basic Modal" @ok="handleOk">
      <DataDisplay :data="data" /><ExportButton
    /></a-modal>
  </div>
</template>
<script setup>
import DataDisplay from "./DataDisplay.vue";
import ExportButton from "./ExportButton.vue";
import { ref, defineProps } from "vue";

const open = ref(false);

const props = defineProps(["data"]);

const showModal = () => {
  open.value = true;
};
const handleOk = (e) => {
  console.log(e);
  open.value = false;
};
</script>
