<template>
  <div class="justify-between">
    <label class="flex mr-4 items-center flex-nowrap">
      <input type="radio" name="show" value="all" checked @change="filter" />
      <span class="ml-1 whitespace-nowrap">Show All</span>
    </label>
    <label class="flex mr-4 items-center flex-nowrap">
      <input type="radio" name="show" value="today" @change="filter" />
      <span class="ml-1 whitespace-nowrap">Due Today</span>
    </label>
    <label class="flex mr-4 items-center flex-nowrap">
      <input type="radio" name="show" value="past" @change="filter" />
      <span class="ml-1 whitespace-nowrap">Past Due</span>
    </label>
  </div>
</template>

<script setup>
import { defineEmits } from "vue";

const emit = defineEmits(["filter"]);

const filter = (e) => {
  emit("filter", e.target.value);
};
</script>
