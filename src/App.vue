<script setup lang="ts">
import APagination from "./components/pagination/APagination.vue";
import InputNumber from "./components/pagination/InputNumber.vue";

import { ref } from "vue";

const currentPage = ref(1);

function onCurrentPageUpdated(value: number) {
  currentPage.value = value;
}
</script>

<template>
  <APagination
    @update:currentPage="onCurrentPageUpdated"
    v-model="currentPage"
  />
  <hr />
  <div>현재페이지 {{ currentPage }}</div>
  <InputNumber v-model="currentPage" />
</template>

<style scoped></style>
