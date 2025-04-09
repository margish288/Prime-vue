<template>
  <div class="card">
    <h1>Single Sort & Multi Sort</h1>

    <DataTable
      :value="products"
      sortMode="multiple"
      tableStyle="min-width: 50rem"
      showGridlines="true"
    >
      <Column field="name" header="Name" sortable style="width: 25%"></Column>
      <Column
        field="department"
        header="Department"
        sortable
        style="width: 25%"
      ></Column>
      <Column
        field="salary"
        header="Salary"
        sortable
        style="width: 25%"
      ></Column>
      <Column field="id" header="ID" sortable style="width: 25%"></Column>
    </DataTable>
  </div>
</template>

<script setup>
import { ref } from "vue";

const products = ref([
  { id: 101, name: "Alice", department: "DEVELOPER", salary: 80000 },
  { id: 102, name: "Bob", department: "HR", salary: 75000 },
  { id: 103, name: "Charlie", department: "OPERATIONS", salary: 85000 },
  { id: 104, name: "David", department: "TESTING", salary: 90000 },
  { id: 105, name: "Eve", department: "DEVELOPER", salary: 80000 },
]);
</script>
