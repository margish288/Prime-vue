<!-- Dynamic Columns, Pagination, Multi Sort, Multi Filter, Add/Remove Columns -->
<template>
  <Tabs value="0">
    <TabList>
      <Tab value="0">Dynamic</Tab>
      <Tab value="1">Pagination</Tab>
      <Tab value="2">Sort</Tab>
      <Tab value="3">Filter</Tab>
      <Tab value="4">Add Remove Column</Tab>
    </TabList>
    <TabPanels>
      <TabPanel value="0">
        <Dynamic />
      </TabPanel>
      <TabPanel value="1">
        <Pagination />
      </TabPanel>
      <TabPanel value="2">
        <Sort />
      </TabPanel>
      <TabPanel value="3">
        <Filter />
      </TabPanel>
      <TabPanel value="4">
        <AddRemove />
      </TabPanel>
    </TabPanels>
  </Tabs>
</template>

<script setup></script>
