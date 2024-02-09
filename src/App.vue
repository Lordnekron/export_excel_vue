<template>
  <div>
    <h1>Vue Excel Export App</h1>
    <Modal :columns="d.columns" :data="e.items" :title="d.title" />
  </div>
</template>

<script setup>
import Modal from "./components/Modal.vue";
</script>
