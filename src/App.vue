<template>
  <div>
    <h1>Vue Excel Export App</h1>
    <Modal :data="d.columns" />
  </div>
</template>

<script setup>
import DataDisplay from "./components/DataDisplay.vue";
import Modal from "./components/Modal.vue";

const components = {
  DataDisplay,
  Modal,
};

const d = {
  title: "Филиалы",
  columns: [
    {
      title: "Название",
      dataIndex: "name",
      sorter: true,
      minWidth: 100,
      width: "40%",
      filterType: "text",
    },
    {
      title: "Предприятие",
      dataIndex: "company_id",
      sorter: true,
      minWidth: 100,
      width: "40%",
      filterType: "text",
    },
  ],
  editRule: "d.locations.edit",
  actions: ["edit", "delete"],
  entityName: "filials",
  createBtn: true,
  values: {
    company_id: (column, text, record) => {
      return record.company.name;
    },
  },
  relations: ["company"],
  expandData: [
    {
      name: "id",
      value: (record) => {
        return record.id;
      },
    },
    {
      name: "Название",
      value: "name",
    },
    {
      name: "Предприятие",
      value: (record) => {
        return record.company.name;
      },
    },
  ],
};
</script>
