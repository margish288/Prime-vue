<template>
  <div>
    <h1>Pagination</h1>

    <DataTable
      :value="products"
      tableStyle="min-width: 50rem"
      showGridlines="true"
      paginator
      :rows="5"
      paginatorTemplate="FirstPageLink PrevPageLink PageLinks NextPageLink LastPageLink CurrentPageReport RowsPerPageDropdown"
      currentPageReportTemplate="Showing {first} to {last} of {totalRecords} entries"
      :rowsPerPageOptions="[5, 10, 20]"
    >
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
    { code: "K", name: "Product K", category: "Category 11", quantity: 110 },
    { code: "L", name: "Product L", category: "Category 12", quantity: 120 },
    { code: "M", name: "Product M", category: "Category 13", quantity: 130 },
    { code: "N", name: "Product N", category: "Category 14", quantity: 140 },
    { code: "O", name: "Product O", category: "Category 15", quantity: 150 },
    { code: "P", name: "Product P", category: "Category 16", quantity: 160 },
    { code: "Q", name: "Product Q", category: "Category 17", quantity: 170 },
    { code: "R", name: "Product R", category: "Category 18", quantity: 180 },
    { code: "S", name: "Product S", category: "Category 19", quantity: 190 },
    { code: "T", name: "Product T", category: "Category 20", quantity: 200 },
    { code: "U", name: "Product U", category: "Category 21", quantity: 210 },
    { code: "V", name: "Product V", category: "Category 22", quantity: 220 },
    { code: "W", name: "Product W", category: "Category 23", quantity: 230 },
    { code: "X", name: "Product X", category: "Category 24", quantity: 240 },
    { code: "Y", name: "Product Y", category: "Category 25", quantity: 250 },
    { code: "Z", name: "Product Z", category: "Category 26", quantity: 260 },
    { code: "AA", name: "Product AA", category: "Category 27", quantity: 270 },
    { code: "AB", name: "Product AB", category: "Category 28", quantity: 280 },
    { code: "AC", name: "Product AC", category: "Category 29", quantity: 290 },
    { code: "AD", name: "Product AD", category: "Category 30", quantity: 300 },
    { code: "AE", name: "Product AE", category: "Category 31", quantity: 310 },
    { code: "AF", name: "Product AF", category: "Category 32", quantity: 320 },
    { code: "AG", name: "Product AG", category: "Category 33", quantity: 330 },
    { code: "AH", name: "Product AH", category: "Category 34", quantity: 340 },
    { code: "AI", name: "Product AI", category: "Category 35", quantity: 350 },
    { code: "AJ", name: "Product AJ", category: "Category 36", quantity: 360 },
    { code: "AK", name: "Product AK", category: "Category 37", quantity: 370 },
    { code: "AL", name: "Product AL", category: "Category 38", quantity: 380 },
    { code: "AM", name: "Product AM", category: "Category 39", quantity: 390 },
    { code: "AN", name: "Product AN", category: "Category 40", quantity: 400 },
    { code: "AO", name: "Product AO", category: "Category 41", quantity: 410 },
    { code: "AP", name: "Product AP", category: "Category 42", quantity: 420 },
    { code: "AQ", name: "Product AQ", category: "Category 43", quantity: 430 },
    { code: "AR", name: "Product AR", category: " Category 44", quantity: 440 },
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
