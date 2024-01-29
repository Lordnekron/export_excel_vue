<template>
  <div>
    <h2>Data Display Component</h2>
    <a-checkbox
      v-model:checked="state.checkAll"
      :indeterminate="state.indeterminate"
      @change="onCheckAllChange"
    >
      Check all
    </a-checkbox>

    <a-checkbox-group v-model:value="state.checkedList" style="width: 100%">
      <a-list size="small" bordered :data-source="columns">
        <template #renderItem="{ item, index }">
          <a-list-item>
            <a-checkbox :value="item" v-model:checked="checked">
              {{ item }}
            </a-checkbox>
          </a-list-item>
        </template>
      </a-list>
    </a-checkbox-group>
  </div>
  <ExportButton :data="data" />
</template>

<script setup>
import { ref, defineProps, reactive, watch } from "vue";
import ExportButton from "./ExportButton.vue";
const checked = ref(false);
const props = defineProps(["data"]);
let columns = [];
for (const item of props.data) {
  columns.push(item.title);
}
console.log(columns);
const plainOptions = columns;

const state = reactive({
  indeterminate: false,
  checkAll: false,
});

const onCheckAllChange = (e) => {
  Object.assign(state, {
    checkedList: e.target.checked ? plainOptions : [],
    indeterminate: false,
  });
};
watch(
  () => state.checkedList,
  (val) => {
    state.indeterminate = !!val.length && val.length < plainOptions.length;
    state.checkAll = val.length === plainOptions.length;
  }
);
</script>
