<template>
  <div>
    <a-checkbox
      v-model:checked="state.checkAll"
      :indeterminate="state.indeterminate"
      @change="onCheckAllChange"
    >
      Выделить всё
    </a-checkbox>
    <a-checkbox-group
      v-model:value="state.checkedList"
      @change="doCheck"
      style="width: 100%"
    >
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
</template>

<script setup>
import { ref, defineProps, reactive, watch, defineEmits } from "vue";
const checked = ref(false);
const checkElement = [];
const props = defineProps(["columns", "data", "title"]);
const emit = defineEmits(["onCheckedList"]);

let columns = [];
for (const item of props.columns) {
  columns.push(item.title);
}

const plainOptions = columns;

const state = reactive({
  indeterminate: false,
  checkAll: false,
  checkedList: checkElement,
});

const doCheck = () => {
  emit("onCheckedList", state.checkedList);
};

const onCheckAllChange = (e) => {
  Object.assign(state, {
    checkedList: e.target.checked ? plainOptions : [],
    indeterminate: false,
  });
  emit("onCheckedList", state.checkedList);
};
watch(
  () => state.checkedList,
  (val) => {
    state.indeterminate = !!val.length && val.length < plainOptions.length;
    state.checkAll = val.length === plainOptions.length;
  }
);
</script>
