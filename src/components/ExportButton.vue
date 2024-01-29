<template>
  <div>
    <a-button type="primary" @click="exportToExcel">Export to Excel</a-button>
  </div>
</template>

<script>
import * as XLSX from "xlsx";

export default {
  methods: {
    exportToExcel() {
      const ws = XLSX.utils.json_to_sheet(this.data);

      // Настройка размера ячеек для каждой колонки
      const columns = [];
      this.data.forEach((item) => {
        Object.keys(item).forEach((key) => {
          if (!columns.includes(key)) {
            columns.push(key);
          }
        });
      });

      columns.forEach((column, index) => {
        const maxLength = Math.max(
          ...this.data.map((row) => String(row[column]).length)
        );
        const columnWidth = Math.min(25, Math.max(10, maxLength * 1.2)); // Ограничиваем ширину до 25 символов, но минимум 10 символов

        ws["!cols"] = ws["!cols"] || [];
        ws["!cols"][index] = { wch: columnWidth };
      });

      const wb = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(wb, ws, "Data");
      XLSX.writeFile(wb, "data.xlsx");
    },
  },
  props: {
    data: Array,
  },
};
</script>
