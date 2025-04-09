<template>
  <div>
    <h1>Add or Remove Columns</h1>
    <DataTable :value="products" tableStyle="min-width: 50rem" showGridlines>
      <template #header>
        <div style="text-align: left">
          <MultiSelect
            :modelValue="selectedColumns"
            :options="columns"
            optionLabel="header"
            @update:modelValue="onToggle"
            display="chip"
            placeholder="Select Columns"
          />
        </div>
      </template>
      <Column field="code" header="Code" />
      <Column
        v-for="(col, index) of selectedColumns"
        :field="col.field"
        :header="col.header"
        :key="col.field + '_' + index"
      ></Column>
    </DataTable>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

onMounted(() => {
  products.value = [
    {
      code: "A1001",
      name: "Product 1",
      category: "Category 1",
      quantity: 10,
      price: 100,
      stock: 50,
      rating: 4.5,
      status: "Available",
      action: "Edit",
    },
    {
      code: "A1002",
      name: "Product 2",
      category: "Category 2",
      quantity: 20,
      price: 200,
      stock: 30,
      rating: 3.5,
      status: "Out of Stock",
      action: "Edit",
    },
    {
      code: "A1003",
      name: "Product 3",
      category: "Category 3",
      quantity: 15,
      price: 150,
      stock: 20,
      rating: 4.0,
      status: "Available",
      action: "Edit",
    },
    {
      code: "A1004",
      name: "Product 4",
      category: "Category 4",
      quantity: 5,
      price: 50,
      stock: 10,
      rating: 2.5,
      status: "Out of Stock",
      action: "Edit",
    },
    {
      code: "A1005",
      name: "Product 5",
      category: "Category 5",
      quantity: 8,
      price: 80,
      stock: 15,
      rating: 3.0,
      status: "Available",
      action: "Edit",
    },
  ];
});

const columns = ref([
  { field: "name", header: "Name" },
  { field: "category", header: "Category" },
  { field: "quantity", header: "Quantity" },
  { field: "price", header: "Price" },
  { field: "stock", header: "Stock" },
  { field: "rating", header: "Rating" },
  { field: "status", header: "Status" },
  { field: "action", header: "Action" },
]);
const selectedColumns = ref(columns.value);
const products = ref();
const onToggle = (val) => {
  selectedColumns.value = columns.value.filter((col) => val.includes(col));
};
</script>
