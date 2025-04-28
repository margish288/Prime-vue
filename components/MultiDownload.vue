<template>
  <div>
    <h1>Single-Multi Selection / Page / All download (CSV)</h1>

    <DataTable
      ref="dgRef"
      :value="products"
      tableStyle="min-width: 50rem"
      showGridlines="true"
      paginator
      v-model:selection="selectedProducts"
      selectionMode="multiple"
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

    <div style="display: flex; gap: 1rem; margin-top: 1rem">
      <Select
        v-model="selectedDownloadOption"
        :options="downloadOptions"
        optionLabel="name"
        placeholder="Select Download Option"
        class="w-full md:w-56"
      />
      <Button
        class="p-button-outlined"
        @click="downloadData(selectedDownloadOption?.code)"
      >
        {{
          selectedDownloadOption?.code !== "all"
            ? "Download Selected"
            : "Download All"
        }}
      </Button>
    </div>
    <Toast />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useToast } from "primevue/usetoast";
const toast = useToast();
const selectedProducts = ref(null);
const dgRef = ref(null);
const products = ref();

const selectedDownloadOption = ref();
const downloadOptions = ref([
  { name: "Single", code: "single" },
  { name: "Multiple", code: "multi" },
  { name: "Current Page", code: "page" },
  { name: "All", code: "all" },
]);

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
    { code: "AS", name: "Product AS", category: "Category 45", quantity: 450 },
    { code: "AT", name: "Product AT", category: "Category 46", quantity: 460 },
    { code: "AU", name: "Product AU", category: "Category 47", quantity: 470 },
    { code: "AV", name: "Product AV", category: "Category 48", quantity: 480 },
    { code: "AW", name: "Product AW", category: "Category 49", quantity: 490 },
    { code: "AX", name: "Product AX", category: "Category 50", quantity: 500 },
    { code: "AY", name: "Product AY", category: "Category 51", quantity: 510 },
    { code: "AZ", name: "Product AZ", category: "Category 52", quantity: 520 },
    { code: "BA", name: "Product BA", category: "Category 53", quantity: 530 },
    { code: "BB", name: "Product BB", category: "Category 54", quantity: 540 },
    { code: "BC", name: "Product BC", category: "Category 55", quantity: 550 },
    { code: "BD", name: "Product BD", category: "Category 56", quantity: 560 },
    { code: "BE", name: "Product BE", category: "Category 57", quantity: 570 },
    { code: "BF", name: "Product BF", category: "Category 58", quantity: 580 },
    { code: "BG", name: "Product BG", category: "Category 59", quantity: 590 },
    { code: "BH", name: "Product BH", category: "Category 60", quantity: 600 },
    { code: "BI", name: "Product BI", category: "Category 61", quantity: 610 },
    { code: "BJ", name: "Product BJ", category: "Category 62", quantity: 620 },
    { code: "BK", name: "Product BK", category: "Category 63", quantity: 630 },
    { code: "BL", name: "Product BL", category: "Category 64", quantity: 640 },
    { code: "BM", name: "Product BM", category: "Category 65", quantity: 650 },
    { code: "BN", name: "Product BN", category: "Category 66", quantity: 660 },
    { code: "BO", name: "Product BO", category: "Category 67", quantity: 670 },
    { code: "BP", name: "Product BP", category: "Category 68", quantity: 680 },
    { code: "BQ", name: "Product BQ", category: "Category 69", quantity: 690 },
    { code: "BR", name: "Product BR", category: "Category 70", quantity: 700 },
    { code: "BS", name: "Product BS", category: "Category 71", quantity: 710 },
    { code: "BT", name: "Product BT", category: "Category 72", quantity: 720 },
    { code: "BU", name: "Product BU", category: "Category 73", quantity: 730 },
    { code: "BV", name: "Product BV", category: "Category 74", quantity: 740 },
    { code: "BW", name: "Product BW", category: "Category 75", quantity: 750 },
    { code: "BX", name: "Product BX", category: "Category 76", quantity: 760 },
    { code: "BY", name: "Product BY", category: "Category 77", quantity: 770 },
    { code: "BZ", name: "Product BZ", category: "Category 78", quantity: 780 },
    { code: "CA", name: "Product CA", category: "Category 79", quantity: 790 },
    { code: "CB", name: "Product CB", category: "Category 80", quantity: 800 },
    { code: "CC", name: "Product CC", category: "Category 81", quantity: 810 },
    { code: "CD", name: "Product CD", category: "Category 82", quantity: 820 },
    { code: "CE", name: "Product CE", category: "Category 83", quantity: 830 },
    { code: "CF", name: "Product CF", category: "Category 84", quantity: 840 },
    { code: "CG", name: "Product CG", category: "Category 85", quantity: 850 },
    { code: "CH", name: "Product CH", category: "Category 86", quantity: 860 },
    { code: "CI", name: "Product CI", category: "Category 87", quantity: 870 },
    { code: "CJ", name: "Product CJ", category: "Category 88", quantity: 880 },
    { code: "CK", name: "Product CK", category: "Category 89", quantity: 890 },
    { code: "CL", name: "Product CL", category: "Category 90", quantity: 900 },
    { code: "CM", name: "Product CM", category: "Category 91", quantity: 910 },
    { code: "CN", name: "Product CN", category: "Category 92", quantity: 920 },
    { code: "CO", name: "Product CO", category: "Category 93", quantity: 930 },
    { code: "CP", name: "Product CP", category: "Category 94", quantity: 940 },
    { code: "CQ", name: "Product CQ", category: "Category 95", quantity: 950 },
    { code: "CR", name: "Product CR", category: " Category 96", quantity: 960 },
  ];
});

const downloadData = (type) => {
  const isSelectionDownload = ["single", "multi"].includes(type);
  if (isSelectionDownload) {
    if (selectedProducts?.value?.length) {
      download(true);
    } else {
      toast.add({
        severity: "warn",
        summary: "No Products Selected",
        detail: "Please select products to download.",
        life: 3000,
      });
    }
  } else if (type === "page") {
    downloadCurrentPage();
  } else {
    download();
  }
};

const download = (isSelectionOnly) => {
  console.log(isSelectionOnly ? "Download" : "Download All");
  dgRef.value.exportCSV({
    selectionOnly: isSelectionOnly,
  });
};

const downloadCurrentPage = () => {
  console.log("Download Current Page");
  const { first, rows } = dgRef.value;
  const currentPageData = products.value.slice(first, first + rows);

  if (!currentPageData.length) {
    toast.add({
      severity: "warn",
      summary: "No Products on Current Page",
      detail: "There are no products to download on this page.",
      life: 3000,
    });
    return;
  }

  let csv = columns.value.map((col) => col.header).join(",") + "\n";

  currentPageData.forEach((item) => {
    let row = columns.value
      .map((col) => {
        let val = item[col.field];
        if (typeof val === "string") {
          val = `"${val.replace(/"/g, '""')}"`;
        }
        return val;
      })
      .join(",");
    csv += row + "\n";
  });

  // Trigger download
  const blob = new Blob([csv], { type: "text/csv;charset=utf-8;" });
  const url = URL.createObjectURL(blob);
  const link = document.createElement("a");
  link.href = url;
  link.setAttribute("download", "download-single-page.csv");
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};

const columns = ref([
  { field: "code", header: "Code" },
  { field: "name", header: "Name" },
  { field: "category", header: "Category" },
  { field: "quantity", header: "Quantity" },
]);
</script>

<style scoped></style>
