<template>
  <div class="bg-white relative border rounded-lg">
    <div class="flex items-center justify-between">
      <SearchForm @search="handelSearch" />
      <div class="flex items-center justify-end text-sm font-semibold">
        <FilterRadio @filter="handleFilterRadio" />
        <FilterDropdown :items="props.items" @filter="handleCheckboxFilter" />
      </div>
    </div>
    <table class="w-full text-sm text-left text-gray-500">
      <thead class="text-xs text-gray-700 uppercase bg-gray-200">
        <th class="px-4 py-3">ID</th>
        <th class="px-4 py-3">Assigned To</th>
        <th class="px-4 py-3">Status</th>
        <th class="px-4 py-3">Title</th>
        <th class="px-4 py-3">
          <span class="sr-only">Actions</span>
        </th>
      </thead>
      <tbody>
        <tr v-for="item in filteredItems" :key="item.id" class="border-b">
          <td class="px-4 py-3 font-medium text-gray-900">{{ item.id }}</td>
          <td class="px-4 py-3 font-medium text-gray-900">
            {{ item.user.name }}
          </td>
          <td class="px-4 py-3">{{ item.status }}</td>
          <td class="px-4 py-3">{{ item.title }}</td>
          <td class="px-4 py-3">{{ item.due_at }}</td>
          <td class="px-4 py-3 flex items-center justify-end">
            <a href="#" class="text-indigo-500 hover:underline">details</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { computed, ref } from "vue";
import FilterDropdown from "./FilterDropdown.vue";
import FilterRadio from "./FilterRadio.vue";
import SearchForm from "./SearchForm.vue";

const searchFilter = ref("");
const radioFilter = ref("");
const statusesFilter = ref([]);

const props = defineProps({
  items: {
    type: Array,
    required: true,
  },
});
const filteredItems = computed(() => {
  let items = props.items;

  switch (radioFilter.value) {
    case "today":
      items = items.filter(
        (item) => new Date(item.due_at).getTime() === new Date().getTime()
      );
      break;
    case "past":
      items = items.filter((item) => new Date(item.due_at) < new Date());
  }

  if (statusesFilter.value.length > 0) {
    items = items.filter((item) => statusesFilter.value.includes(item.status));
  }

  if (searchFilter.value.trim().toLowerCase() !== "") {
    items = items.filter(
      (item) =>
        item.title.toLowerCase().includes(searchFilter.value) ||
        item.user.name.toLowerCase().includes(searchFilter.value)
    );
  }
  return items;
});
const handelSearch = (search) => {
  searchFilter.value = search;
};
const handleFilterRadio = (filter) => {
  radioFilter.value = filter;
  console.log("Selected filter:", filter);
};
const handleCheckboxFilter = (filter) => {
  if (statusesFilter.value.includes(filter)) {
    return statusesFilter.value.splice(statusesFilter.value.indexOf(filter), 1);
  }
  statusesFilter.value.push(filter);
};
</script>
