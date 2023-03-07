<script setup lang="ts">
import APagination from "./components/pagination/APagination.vue";
import InputNumber from "./components/pagination/InputNumber.vue";
import APagesToShow from "./components/pagination/APagesToShow.vue";
import APageCount from "./components/pagination/APageCount.vue";

import { ref } from "vue";

const currentPage = ref<number>(1);
const pagesToShow = ref<number>(1);
const pageCount = ref<number>(1);

function onCurrentPageUpdated(value: number) {
  currentPage.value = value;
}

function onPagesToShowUpdated(value: number) {
  pagesToShow.value = value;
}
function onPageCountUpdated(value: number) {
  pageCount.value = value;
}
function onChangedPage(page: number) {
  console.log(page);
}
</script>

<template>
  <APagination
    @update:currentPage="onCurrentPageUpdated"
    @update:pagesToShow="onPagesToShowUpdated"
    @update:pageCount="onPageCountUpdated"
    @click="onChangedPage"
    v-model="currentPage"
    :pageShowValue="pagesToShow"
    :pageCountValue="pageCount"
  />
  <hr />
  <InputNumber v-model.number="currentPage" />
  <hr />
  <APagesToShow v-model.number="pagesToShow" />
  <hr />
  <APageCount v-model.number="pageCount" />
</template>

<style scoped></style>
