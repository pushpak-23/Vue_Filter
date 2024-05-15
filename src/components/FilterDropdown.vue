<template>
  <div class="relative flex items-center w-full px-4">
    <button
      @click="show = !show"
      class="relative px-8 py-2 rounded-md bg-white isolation-auto z-10 border-2 border-red-700 before:absolute before:w-full before:transition-all before:duration-700 before:hover:w-full hover:text-white before:-right-full before:hover:right-0 before:rounded-full before:bg-[#A12347] before:-z-10 before:aspect-square before:hover:scale-150 overflow-hidden before:hover:duration-700 inline-flex items-center justify-center px-4 py-3 text-sm font-semibold text-black bg-white border border-gray-200 rounded-lg shadow-sm gap-x-2 hover:bg-gray-50 disabled:opacity-50 disabled:pointer-events-none"
    >
      Filter
    </button>
    <div
      v-if="show"
      class="absolute top-12 right-0 z-10 w-48 p-3 bg-white rounded-lg shadow"
    >
      <h6 class="mb-3 text-sm font-medium text-gray-900">Status</h6>
      <ul class="space-y-2 text-sm">
        <li v-for="(status, index) in statuses" :key="index">
          <input
            :id="`filter_option_${index}`"
            type="checkbox"
            :value="status"
            @change="filter"
            class="w-4 h-4 bg-gray-100 border-gray-300 rounded text-lg"
          />
          <label
            :for="`filter_option_${index}`"
            class="ml-2 text-sm font-medium text-gray-900"
            >{{ status }}</label
          >
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from "vue";

const show = ref(false);
const props = defineProps({
  items: {
    type: Array,
    required: true,
  },
});
const emit = defineEmits("filter");

const statuses = computed(() => {
  return [...new Set(props.items.map((item) => item.status))];
});

const filter = (e) => {
  emit("filter", e.target.value);
};
</script>

<style lang="scss" scoped></style>
