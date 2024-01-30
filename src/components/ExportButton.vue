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
      console.log(this.columns);
      const filteredData = this.columns.filter((row) => {
        return this.checked.includes(row.title);
      });

      const columnTitles = filteredData.map((column) => column.title);
      const excelData = [columnTitles];

      this.data.forEach((item) => {
        const rowData = filteredData.map((column) => {
          const dataIndex = column.dataIndex;
          if (dataIndex in item) {
            return item[dataIndex];
          } else {
            return "";
          }
        });
        excelData.push(rowData);
      });

      const ws = XLSX.utils.aoa_to_sheet(excelData);

      // Настройка размера ячеек для каждой колонки
      filteredData.forEach((column, index) => {
        const maxLength = Math.max(
          ...excelData.slice(1).map((row) => String(row[index]).length)
        );
        const columnWidth = Math.min(25, Math.max(10, maxLength * 1.2));

        ws["!cols"] = ws["!cols"] || [];
        ws["!cols"][index] = { wch: columnWidth };
      });

      const wb = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(wb, ws, "Data");
      XLSX.writeFile(wb, "data.xlsx");
    },
  },
  props: {
    columns: Array,
    checked: Array,
    data: Array,
  },
};
</script>
