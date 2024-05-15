<template>
  <div class="p-8 bg-gray-100 min-h-screen">
    <DataTable :items="items" />
  </div>
  <RouterView />
</template>

<script setup>
import { onMounted, ref } from "vue";
import DataTable from "./components/DataTable.vue";

const items = ref([]);

onMounted(async () => {
  try {
    const response = await fetch("http://localhost:3000/items");
    if (!response.ok) {
      throw new Error("Failed to fetch data");
    }
    items.value = await response.json();
  } catch (error) {
    console.error("Error fetching data:", error.message);
  }
});
</script>
