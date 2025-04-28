<template>
  <div class="card">
    <DataTable
      v-model:filters="filters"
      ref="dgRef"
      :value="customers"
      paginator
      showGridlines
      selectionMode="multiple"
      :metaKeySelection="metaKey"
      :rows="10"
      dataKey="id"
      filterDisplay="menu"
      :loading="loading"
      :globalFilterFields="[
        'name',
        'country.name',
        'representative.name',
        'balance',
        'status',
      ]"
    >
      <template #header>
        <div style="display: flex; justify-content: space-between">
          <Button
            type="button"
            icon="pi pi-filter-slash"
            label="Clear"
            outlined
            @click="clearFilter()"
          />
          <IconField>
            <InputIcon>
              <i class="pi pi-search" />
            </InputIcon>
            <InputText
              v-model="filters['global'].value"
              placeholder="Keyword Search"
            />
          </IconField>
        </div>
      </template>
      <template #empty> No customers found. </template>
      <template #loading> Loading customers data. Please wait. </template>
      <Column field="name" header="Name" style="min-width: 12rem">
        <template #body="{ data }">
          {{ data.name }}
        </template>
        <template #filter="{ filterModel }">
          <InputText
            v-model="filterModel.value"
            type="text"
            placeholder="Search by name"
          />
        </template>
      </Column>
      <Column
        header="Country"
        filterField="country.name"
        style="min-width: 12rem"
      >
        <template #body="{ data }">
          <div class="flex items-center gap-2">
            <img
              alt="flag"
              src="https://primefaces.org/cdn/primevue/images/flag/flag_placeholder.png"
              :class="`flag flag-${data.country.code}`"
              style="width: 24px"
            />
            <span>{{ data.country.name }}</span>
          </div>
        </template>
        <template #filter="{ filterModel }">
          <InputText
            v-model="filterModel.value"
            type="text"
            placeholder="Search by country"
          />
        </template>
        <template #filterclear="{ filterCallback }">
          <Button
            type="button"
            icon="pi pi-times"
            @click="filterCallback()"
            severity="secondary"
          ></Button>
        </template>
        <template #filterapply="{ filterCallback }">
          <Button
            type="button"
            icon="pi pi-check"
            @click="filterCallback()"
            severity="success"
          ></Button>
        </template>
        <template #filterfooter>
          <div class="px-4 pt-0 pb-4 text-center">Customized Buttons</div>
        </template>
      </Column>
      <Column
        header="Agent"
        filterField="representative"
        :showFilterMatchModes="false"
        :filterMenuStyle="{ width: '14rem' }"
        style="min-width: 14rem"
      >
        <template #body="{ data }">
          <div style="display: flex; align-items: center; gap: 0.5rem">
            <img
              :alt="data.representative.name"
              :src="`https://primefaces.org/cdn/primevue/images/avatar/${data.representative.image}`"
              style="width: 32px"
            />
            <span>{{ data.representative.name }}</span>
          </div>
        </template>
        <template #filter="{ filterModel }">
          <MultiSelect
            v-model="filterModel.value"
            :options="representatives"
            optionLabel="name"
            placeholder="Any"
          >
            <template #option="slotProps">
              <div style="display: flex; align-items: center; gap: 0.5rem">
                <img
                  :alt="slotProps.option.name"
                  :src="`https://primefaces.org/cdn/primevue/images/avatar/${slotProps.option.image}`"
                  style="width: 32px"
                />
                <span>{{ slotProps.option.name }}</span>
              </div>
            </template>
          </MultiSelect>
        </template>
      </Column>
      <Column
        header="Date"
        filterField="date"
        dataType="date"
        style="min-width: 10rem"
      >
        <template #body="{ data }">
          {{ formatDate(data.date) }}
        </template>
        <template #filter="{ filterModel }">
          <DatePicker
            v-model="filterModel.value"
            dateFormat="mm/dd/yy"
            placeholder="mm/dd/yyyy"
          />
        </template>
      </Column>
      <Column
        header="Balance"
        filterField="balance"
        dataType="numeric"
        style="min-width: 10rem"
      >
        <template #body="{ data }">
          {{ formatCurrency(data.balance) }}
        </template>
        <template #filter="{ filterModel }">
          <InputNumber
            v-model="filterModel.value"
            mode="currency"
            currency="USD"
            locale="en-US"
          />
        </template>
      </Column>
      <Column
        header="Status"
        field="status"
        :filterMenuStyle="{ width: '14rem' }"
        style="min-width: 12rem"
      >
        <template #body="{ data }">
          <Tag :value="data.status" :severity="getSeverity(data.status)" />
        </template>
        <template #filter="{ filterModel }">
          <Select
            v-model="filterModel.value"
            :options="statuses"
            placeholder="Select One"
            showClear
          >
            <template #option="slotProps">
              <Tag
                :value="slotProps.option"
                :severity="getSeverity(slotProps.option)"
              />
            </template>
          </Select>
        </template>
      </Column>
      <Column
        field="activity"
        header="Activity"
        :showFilterMatchModes="false"
        style="min-width: 12rem"
      >
        <template #body="{ data }">
          <ProgressBar
            :value="data.activity"
            :showValue="false"
            style="height: 6px"
          ></ProgressBar>
        </template>
        <template #filter="{ filterModel }">
          <Slider v-model="filterModel.value" range class="m-4"></Slider>
          <div class="flex items-center justify-between px-2">
            <span>{{ filterModel.value ? filterModel.value[0] : 0 }}</span>
            <span>{{ filterModel.value ? filterModel.value[1] : 100 }}</span>
          </div>
        </template>
      </Column>
      <Column
        field="verified"
        header="Verified"
        dataType="boolean"
        bodyClass="text-center"
        style="min-width: 8rem"
      >
        <template #body="{ data }">
          <i
            class="pi"
            :class="{
              'pi-check-circle text-green-500 ': data.verified,
              'pi-times-circle text-red-500': !data.verified,
            }"
          ></i>
        </template>
        <template #filter="{ filterModel }">
          <label for="verified-filter" class="font-bold"> Verified </label>
          <Checkbox
            v-model="filterModel.value"
            :indeterminate="filterModel.value === null"
            binary
            inputId="verified-filter"
          />
        </template>
      </Column>
    </DataTable>

    <Button
      type="button"
      icon="pi pi-filter-slash"
      label="Download CSV"
      outlined
      @click="download()"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { FilterMatchMode, FilterOperator } from "@primevue/core/api";

const customers = ref();
const filters = ref();
const representatives = ref([
  { name: "Amy Elsner", image: "amyelsner.png" },
  { name: "Anna Fali", image: "annafali.png" },
  { name: "Asiya Javayant", image: "asiyajavayant.png" },
  { name: "Bernardo Dominic", image: "bernardodominic.png" },
  { name: "Elwin Sharvill", image: "elwinsharvill.png" },
  { name: "Ioni Bowcher", image: "ionibowcher.png" },
  { name: "Ivan Magalhaes", image: "ivanmagalhaes.png" },
  { name: "Onyama Limba", image: "onyamalimba.png" },
  { name: "Stephen Shaw", image: "stephenshaw.png" },
  { name: "XuXue Feng", image: "xuxuefeng.png" },
]);
const statuses = ref([
  "unqualified",
  "qualified",
  "new",
  "negotiation",
  "renewal",
  "proposal",
]);
const loading = ref(true);
const dgRef = ref(null);

const metaKey = ref(true);
const selectedCustomers = ref(null);

onMounted(() => {
  // CustomerService.getCustomersMedium().then((data) => {
  //   customers.value = getCustomers(data);
  // });
  customers.value = [
    {
      name: "Amy Elsner",
      country: { name: "United States", code: "US" },
      representative: representatives.value[0],
      date: new Date("2023-10-01"),
      balance: 100000,
      status: "qualified",
      activity: 100,
      verified: true,
    },
    {
      name: "Anna Fali",
      country: { name: "United States", code: "US" },
      representative: representatives.value[1],
      date: new Date("2023-10-02"),
      balance: 200000,
      status: "unqualified",
      activity: 80,
      verified: false,
    },
    {
      name: "Asiya Javayant",
      country: { name: "United States", code: "US" },
      representative: representatives.value[2],
      date: new Date("2023-10-03"),
      balance: 300000,
      status: "new",
      activity: 60,
      verified: true,
    },
    {
      name: "Bernardo Dominic",
      country: { name: "United States", code: "US" },
      representative: representatives.value[3],
      date: new Date("2023-10-04"),
      balance: 400000,
      status: "negotiation",
      activity: 40,
      verified: false,
    },
    {
      name: "Elwin Sharvill",
      country: { name: "United States", code: "US" },
      representative: representatives.value[4],
      date: new Date("2023-10-05"),
      balance: 500000,
      status: "renewal",
      activity: 20,
      verified: true,
    },
    {
      name: "Ioni Bowcher",
      country: { name: "United States", code: "US" },
      representative: representatives.value[5],
      date: new Date("2023-10-06"),
      balance: 600000,
      status: "proposal",
      activity: 10,
      verified: false,
    },
    {
      name: "Ivan Magalhaes",
      country: { name: "United States", code: "US" },
      representative: representatives.value[6],
      date: new Date("2023-10-07"),
      balance: 700000,
      status: "qualified",
      activity: 90,
      verified: true,
    },
    {
      name: "Onyama Limba",
      country: { name: "United States", code: "US" },
      representative: representatives.value[7],
      date: new Date("2023-10-08"),
      balance: 800000,
      status: "unqualified",
      activity: 70,
      verified: false,
    },
    {
      name: "Stephen Shaw",
      country: { name: "United States", code: "US" },
      representative: representatives.value[8],
      date: new Date("2023-10-09"),
      balance: 900000,
      status: "new",
      activity: 50,
      verified: true,
    },
    {
      name: "XuXue Feng",
      country: { name: "United States", code: "US" },
      representative: representatives.value[9],
      date: new Date("2023-10-10"),
      balance: 1000000,
      status: "negotiation",
      activity: 30,
      verified: false,
    },
    {
      name: "John Doe",
      country: { name: "United States", code: "US" },
      representative: representatives.value[0],
      date: new Date("2023-10-11"),
      balance: 1100000,
      status: "renewal",
      activity: 20,
      verified: true,
    },
    {
      name: "Jane Smith",
      country: { name: "United States", code: "US" },
      representative: representatives.value[1],
      date: new Date("2023-10-12"),
      balance: 1200000,
      status: "proposal",
      activity: 10,
      verified: false,
    },
    {
      name: "Michael Johnson",
      country: { name: "United States", code: "US" },
      representative: representatives.value[2],
      date: new Date("2023-10-13"),
      balance: 1300000,
      status: "qualified",
      activity: 90,
      verified: true,
    },
    {
      name: "Emily Davis",
      country: { name: "United States", code: "US" },
      representative: representatives.value[3],
      date: new Date("2023-10-14"),
      balance: 1400000,
      status: "unqualified",
      activity: 70,
      verified: false,
    },
    {
      name: "David Brown",
      country: { name: "United States", code: "US" },
      representative: representatives.value[4],
      date: new Date("2023-10-15"),
      balance: 1500000,
      status: "new",
      activity: 50,
      verified: true,
    },
    {
      name: "Sarah Wilson",
      country: { name: "United States", code: "US" },
      representative: representatives.value[5],
      date: new Date("2023-10-16"),
      balance: 1600000,
      status: "negotiation",
      activity: 30,
      verified: false,
    },
    {
      name: "James Taylor",
      country: { name: "United States", code: "US" },
      representative: representatives.value[6],
      date: new Date("2023-10-17"),
      balance: 1700000,
      status: "renewal",
      activity: 20,
      verified: true,
    },
    {
      name: "Linda Anderson",
      country: { name: "United States", code: "US" },
      representative: representatives.value[7],
      date: new Date("2023-10-18"),
      balance: 1800000,
      status: "proposal",
      activity: 10,
      verified: false,
    },
    {
      name: "Robert Martinez",
      country: { name: "United States", code: "US" },
      representative: representatives.value[8],
      date: new Date("2023-10-19"),
      balance: 1900000,
      status: "qualified",
      activity: 90,
      verified: true,
    },
    {
      name: "Patricia Garcia",
      country: { name: "United States", code: "US" },
      representative: representatives.value[9],
      date: new Date("2023-10-20"),
      balance: 2000000,
      status: "unqualified",
      activity: 70,
      verified: false,
    },
    {
      name: "Charles Lee",
      country: { name: "United States", code: "US" },
      representative: representatives.value[0],
      date: new Date("2023-10-21"),
      balance: 2100000,
      status: "new",
      activity: 50,
      verified: true,
    },
    {
      name: "Jessica Wilson",
      country: { name: "United States", code: "US" },
      representative: representatives.value[1],
      date: new Date("2023-10-22"),
      balance: 2200000,
      status: "negotiation",
      activity: 30,
      verified: false,
    },
    {
      name: "Daniel Harris",
      country: { name: "United States", code: "US" },
      representative: representatives.value[2],
      date: new Date("2023-10-23"),
      balance: 2300000,
      status: "renewal",
      activity: 20,
      verified: true,
    },
    {
      name: "Sophia Clark",
      country: { name: "United States", code: "US" },
      representative: representatives.value[3],
      date: new Date("2023-10-24"),
      balance: 2400000,
      status: "proposal",
      activity: 10,
      verified: false,
    },
  ];
  loading.value = false;
});

const initFilters = () => {
  filters.value = {
    global: { value: null, matchMode: FilterMatchMode.CONTAINS },
    name: {
      operator: FilterOperator.AND,
      constraints: [{ value: null, matchMode: FilterMatchMode.STARTS_WITH }],
    },
    "country.name": {
      operator: FilterOperator.AND,
      constraints: [{ value: null, matchMode: FilterMatchMode.STARTS_WITH }],
    },
    representative: { value: null, matchMode: FilterMatchMode.IN },
    date: {
      operator: FilterOperator.AND,
      constraints: [{ value: null, matchMode: FilterMatchMode.DATE_IS }],
    },
    balance: {
      operator: FilterOperator.AND,
      constraints: [{ value: null, matchMode: FilterMatchMode.EQUALS }],
    },
    status: {
      operator: FilterOperator.OR,
      constraints: [{ value: null, matchMode: FilterMatchMode.EQUALS }],
    },
    activity: { value: [0, 100], matchMode: FilterMatchMode.BETWEEN },
    verified: { value: null, matchMode: FilterMatchMode.EQUALS },
  };
};

initFilters();

const download = () => {
  console.log("Download CSV", dgRef.value);
  dgRef.value.exportCSV({
    selectionOnly: true,
  });
};

const formatDate = (value) => {
  return value.toLocaleDateString("en-US", {
    day: "2-digit",
    month: "2-digit",
    year: "numeric",
  });
};
const formatCurrency = (value) => {
  return value.toLocaleString("en-US", { style: "currency", currency: "USD" });
};
const clearFilter = () => {
  initFilters();
};
const getCustomers = (data) => {
  return [...(data || [])].map((d) => {
    d.date = new Date(d.date);

    return d;
  });
};
const getSeverity = (status) => {
  switch (status) {
    case "unqualified":
      return "danger";

    case "qualified":
      return "success";

    case "new":
      return "info";

    case "negotiation":
      return "warn";

    case "renewal":
      return null;
  }
};
</script>
