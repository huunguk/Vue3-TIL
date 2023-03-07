<template>
  <div class="pagination">
    <button @click="leftEndPage" :disabled="currentPage === 1">«</button>
    <button @click="decrementPage" :disabled="currentPage === 1">◁</button>
    <ul>
      <li
        v-for="(pageNumber, index) in visiblePage"
        :key="pageNumber"
        :class="{ active: pageNumber === currentPage }"
      >
        <a
          v-if="pageNumber !== '...'"
          href="#"
          @click.prevent="gotoPage(pageNumber)"
        >
          {{ pageNumber }}
        </a>
        <span v-else>...</span>
      </li>
    </ul>
    <button @click="incrementPage" :disabled="currentPage === pageCount">
      ▷
    </button>
    <button @click="rightEndPage" :disabled="currentPage === pageCount">
      »
    </button>
  </div>
</template>
<script setup lang="ts">
import InputNumber from "./InputNumber.vue";
import { ref, toRefs, computed, watch, onMounted } from "vue";
const currentPage = ref<number>(1); //현재 페이지 번호 추적
const pageCount = ref<number>(20); //전체 페이지 수
const pagesToShow = ref<number>(8); //페이지네이션 길이

const props = defineProps<{
  modelValue: number;
  pageShowValue: number;
  pageCountValue: number;
}>();

const { modelValue, pageShowValue, pageCountValue } = toRefs(props);

const emits = defineEmits<{
  (e: "update:currentPage", value: number): void;
  (e: "update:pagesToShow", value: number): void;
  (e: "update:pageCount", value: number): void;
}>();

const visiblePage = computed(() => {
  const pageToShowHalf: number = Math.floor(pagesToShow.value / 2);
  let startPage: number = currentPage.value - pageToShowHalf + 1;
  let endPage: number = currentPage.value + pageToShowHalf;
  const pages: string[] = [];
  let startArea = pagesToShow.value - 2; // ..., 1

  // if (currentPage.value < startArea) {
  //   for(let i=0; i<startArea; i++) {
  //     pages.push( (i + 1).toString())
  //   }
  //   pages.push('...', props.pageCountValue.toString())
  // } else if () {

  // } else {

  // }

  if (startPage <= 5) {
    startPage = 1;
    endPage = pagesToShow.value;
  }

  if (endPage > pageCount.value) {
    endPage = pageCount.value;
    startPage = endPage - pagesToShow.value + 1;
  }

  for (let i: number = startPage; i <= endPage; i++) {
    pages.push(i);
  }

  if (startPage > 1) {
    pages.unshift("...");
    pages.unshift(1);
    pages.pop();
    pages.pop();
  } else if (startPage < 1) {
    pages.unshift(1);
  }

  if (endPage < pageCount.value) {
    pages.push("...");
    pages.push(pageCount.value);
  } else if (endPage >= pageCount.value) {
    pages.push(pageCount.value - 1);
    pages.push(pageCount.value);
  }

  return pages;
});

onMounted(() => {
  emits("update:pagesToShow", pagesToShow.value);
}),
  watch(currentPage, (_) => {
    emits("update:currentPage", currentPage.value);
  });

watch(modelValue, () => {
  currentPage.value = modelValue.value;
});

function gotoPage(pageNumber: number): void {
  currentPage.value = pageNumber;
}

function decrementPage(): void {
  currentPage.value > 1 && currentPage.value--;
}

function incrementPage(): void {
  currentPage.value < pageCount.value && currentPage.value++;
}

function leftEndPage(): void {
  currentPage.value = 1;
}

function rightEndPage(): void {
  currentPage.value = pageCount.value;
}
</script>

<style scoped>
* {
  list-style: none;
  padding: 0;
}

.pagination {
  display: flex;
  list-style-type: none;
  padding: 0;
  margin-top: 20px;
}

.pagination button {
  font-size: 16px;
  font-weight: 900;
  display: flex;
  align-items: center;
  border: none;
  background: inherit;
  padding: 0px 15px;
  cursor: pointer;
}

.pagination ul {
  display: flex;
  align-items: center;
  justify-content: center;
}

.pagination li {
  width: 40px;
  text-align: center;
  margin: 0 5px;
  font-weight: bold;
}
.pagination li a {
  display: block;
  weight: 40px;
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  text-decoration: none;
  color: #333;
}

.pagination li.active a {
  font-weight: 700;
  border: 1px solid #ccc;
  background-color: #d6e8fe;
}

.pagination span {
  weight: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
