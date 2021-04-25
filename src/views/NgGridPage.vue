<template>
  <div>
    <button @click="getSelectedRows()">Get Selected Rows</button>
    <ag-grid-vue
      class="ag-theme-alpine NgGridPage"
      :columnDefs="columnDefs"
      :rowData="rowData"
      rowSelection="multiple"
      @grid-ready="onGridReady"
    >
    </ag-grid-vue>
  </div>
</template>

<script>
import { AgGridVue } from "ag-grid-vue";

export default {
  name: "NgGridPage",
  data() {
    return {
      columnDefs: null,
      rowData: null,
      gridApi: null,
      columnApi: null,
      autoGroupColumnDef: null,
    };
  },
  components: {
    AgGridVue,
  },
  methods: {
    onGridReady(params) {
      this.gridApi = params.api;
      this.columnApi = params.columnApi;
    },
    getSelectedRows() {
      const selectedNodes = this.gridApi.getSelectedNodes();
      const selectedData = selectedNodes.map((node) => node.data);
      const selectedDataStringPresentation = selectedData
        .map((node) => `${node.make} ${node.model}`)
        .join(", ");
      alert(`Selected nodes: ${selectedDataStringPresentation}`);
    },
  },
  beforeMount() {
    this.columnDefs = [
      { field: "make", sortable: true, filter: true, checkboxSelection: true },
      { field: "model", sortable: true, filter: true },
      { field: "price", sortable: true, filter: true },
    ];

    this.autoGroupColumnDef = {
      headerName: "Model",
      field: "model",
      cellRenderer: "agGroupCellRenderer",
      cellRendererParams: {
        checkbox: true,
      },
    };
    fetch("https://www.ag-grid.com/example-assets/row-data.json")
      .then((result) => result.json())
      .then((rowData) => (this.rowData = rowData));
  },
};
</script>

<style lang="less" scoped>

.NgGridPage {
  width: 650px;
  height: 600px;
  margin: 0 auto;
}
</style>


