<template>
  <div>
    <a-button type="primary" @click="exportToExcel">Экспортировать</a-button>
  </div>
</template>

<script>
import * as XLSX from "xlsx";

export default {
  methods: {
    exportToExcel() {
      // Фильтрация выбранных столбцов
      const filteredColumns = this.columns.filter((column) =>
        this.checked.includes(column.title)
      );
      console.log(this.checked);
      // Формирование заголовков и данных для Excel
      const excelData = [filteredColumns.map((column) => column.title)];
      this.data.forEach((item) => {
        const rowData = filteredColumns.map((column) => {
          const dataIndex = column.dataIndex;
          if (dataIndex in item) {
            let value;
            if (dataIndex.endsWith("_id")) {
              value = item[dataIndex.replace("_id", "")]?.name;
            } else if (dataIndex.endsWith("_by")) {
              value = `${item[dataIndex.replace("_by", "By")]?.f} ${
                item[dataIndex.replace("_by", "By")]?.i
              }`;
            } else if (dataIndex === "clean_type") {
              value =
                column.filters[
                  column.filters.findIndex((e) => e.value === item[dataIndex])
                ].text;
            } else {
              value = item[dataIndex];
            }
            return value !== undefined ? value : ""; // Проверка на undefined
          } else {
            return "";
          }
        });
        excelData.push(rowData);
      });

      // Создание листа Excel
      const ws = XLSX.utils.aoa_to_sheet(excelData);

      // Настройка размера ячеек для каждой колонки
      filteredColumns.forEach((column, index) => {
        const titleLength = String(column.title).length;
        const maxLength = Math.max(
          ...excelData.slice(1).map((row) => String(row[index]).length),
          titleLength
        );
        const columnWidth = Math.min(25, Math.max(10, maxLength * 1.2));

        ws["!cols"] = ws["!cols"] || [];
        ws["!cols"][index] = { wch: columnWidth };
      });

      // Создание книги и запись в файл
      const wb = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(wb, ws, "Data");
      XLSX.writeFile(wb, `${this.title}.xlsx`);
    },
  },
  props: {
    columns: Array,
    checked: Array,
    data: Array,
    title: String,
  },
};
</script>
