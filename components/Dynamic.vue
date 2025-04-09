<template>
  <div>
    <h1>Dynamic Table - re-ordering of columns & rows</h1>

    <DataTable
      :value="products"
      reorderableColumns
      @columnReorder="onColReorder"
      @rowReorder="onRowReorder"
      tableStyle="min-width: 50rem"
      showGridlines
    >
      <Column rowReorder headerStyle="width: 3rem" :reorderableColumn="false" />
      <Column
        v-for="col of columns"
        :field="col.field"
        :header="col.header"
        :key="col.field"
      ></Column>
    </DataTable>
    <Toast />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useToast } from "primevue/usetoast";

onMounted(() => {
  products.value = [
    { code: "A", name: "Product A", category: "Category 1", quantity: 10 },
    { code: "B", name: "Product B", category: "Category 2", quantity: 20 },
    { code: "C", name: "Product C", category: "Category 3", quantity: 30 },
    { code: "D", name: "Product D", category: "Category 4", quantity: 40 },
    { code: "E", name: "Product E", category: "Category 5", quantity: 50 },
    { code: "F", name: "Product F", category: "Category 6", quantity: 60 },
    { code: "G", name: "Product G", category: "Category 7", quantity: 70 },
    { code: "H", name: "Product H", category: "Category 8", quantity: 80 },
    { code: "I", name: "Product I", category: "Category 9", quantity: 90 },
    { code: "J", name: "Product J", category: "Category 10", quantity: 100 },
  ];
});

const toast = useToast();
const columns = ref([
  { field: "code", header: "Code" },
  { field: "name", header: "Name" },
  { field: "category", header: "Category" },
  { field: "quantity", header: "Quantity" },
]);
const products = ref();

const onColReorder = () => {
  toast.add({ severity: "success", summary: "Column Reordered", life: 3000 });
};
const onRowReorder = (event) => {
  products.value = event.value;
  toast.add({ severity: "success", summary: "Rows Reordered", life: 3000 });
};
</script>
