<script setup lang="ts">
import APagination from "./components/pagination/APagination.vue";
import InputNumber from "./components/pagination/InputNumber.vue";
import APagesToShow from "./components/pagination/APagesToShow.vue";
import APageCount from "./components/pagination/APageCount.vue";

import { ref, watch, computed } from "vue";

const currentPage = ref(1);
const currentPageProp = ref(1);
watch(currentPage, () => {
  currentPageProp.value = currentPage.value;
});
const pagesToShow = ref(10);
const pageCount = ref(20);

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
watch(pageCount, () => {
  console.log(typeof pageCount.value);
});
</script>

<template>
  <APagination
    @update:currentPage="onCurrentPageUpdated"
    @update:pagesToShow="onPagesToShowUpdated"
    @update:pageCount="onPageCountUpdated"
    @click="onChangedPage"
    v-model.number="currentPage"
    :pagesToShow="pagesToShow"
    :pageCount="pageCount"
  />
  <hr />
  <InputNumber v-model.number="currentPage" :pageCountValue="pageCount" />
  <hr />
  <APagesToShow v-model.number="pagesToShow" :pageCountValue="pageCount" />
  <hr />
  <APageCount v-model.number="pageCount" :pageShowValue="pagesToShow" />
</template>

<style scoped></style>
