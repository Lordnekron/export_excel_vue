<template>
  <div>
    <a-button type="primary" @click="showModal">Экспорт в Excell</a-button>
    <a-modal v-model:open="open" title="Экспорт в Excell" @ok="handleOk">
      <DataDisplay
        :columns="columns"
        :data="data"
        :title="title"
        @onCheckedList="onCheckedList"
      />
      <template #footer>
        <a-button key="back" @click="handleCancel">Отмена</a-button>
        <ExportButton
          :columns="props.columns"
          :data="props.data"
          :title="title"
          :checked="checkedList"
          @click="handleOk"
        />
      </template>
    </a-modal>
  </div>
</template>
<script setup>
import { ref, defineProps } from "vue";
import DataDisplay from "./DataDisplay.vue";
import ExportButton from "./ExportButton.vue";
const open = ref(false);
let checkedList = [];

const props = defineProps(["columns", "data", "title"]);

const showModal = () => {
  open.value = true;
};
const handleOk = (e) => {
  console.log(e);
  open.value = false;
};
const handleCancel = () => {
  open.value = false;
};

const onCheckedList = (onCheckedList) => {
  console.log(Array.from(onCheckedList));
  checkedList = Array.from(onCheckedList);
};
</script>
