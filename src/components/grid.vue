<template>
  <grid
    :entityName="d.entityName"
    :title="d.title"
    :columns="d.columns"
    :editRule="d.editRule"
    :actions="d.actions"
    :createBtn="d.createBtn"
    :values="d.values"
    :relations="d.relations"
    :expandData="d.expandData"
  />
</template>
<script setup>
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
