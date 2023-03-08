<template>
  <div class="pagination">
    <button @click="leftEndPage" :disabled="currentPage === 1">«</button>
    <button @click="decrementPage" :disabled="currentPage === 1">◁</button>
    <ul>
      <li
        v-for="(pageNumber, index) in visiblePage"
        :key="index"
        :class="{ active: pageNumber === currentPage.toString() }"
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
const currentPage = ref<number>(1);

const props = withDefaults(
  defineProps<{
    modelValue: number;
    pagesToShow?: number;
    pageCount: number;
  }>(),
  { pageShowValue: 11 }
);

const { modelValue, pagesToShow, pageCount } = toRefs(props);

const emits = defineEmits<{
  (e: "update:currentPage", value: number): void;
  (e: "update:pagesToShow", value: number): void;
  (e: "update:pageCount", value: number): void;
  (e: "click", value: number): void;
}>();

const visiblePage = computed(() => {
  const pages: string[] = [];
  let left = pagesToShow.value - 2;
  let right = pageCount.value - left + 1;
  let center = pagesToShow.value - 4;

  if (currentPage.value <= left) {
    for (let i: number = 1; i <= left; i++) {
      pages.push(i.toString());
    }
    pages.push("...", pageCount.value);
  } else if (currentPage.value >= right) {
    pages.push("1", "...");
    for (let i: number = right; i <= pageCount.value; i++) {
      pages.push(i.toString());
    }
  } else {
    pages.push("1", "...");
    if (pagesToShow.value % 2 === 0) {
      for (let i = 0; i < center; i++) {
        pages.push((currentPage.value - center / 2 + i + 1).toString());
      }
    } else {
      for (let i = 0; i < center; i++) {
        pages.push((currentPage.value - Math.floor(center / 2) + i).toString());
      }
    }
    pages.push("...", pageCount.value.toString());
  }

  return pages;
});

watch(currentPage, (_) => {
  emits("update:currentPage", currentPage.value);
});

watch(modelValue, () => {
  currentPage.value = modelValue.value;
});

function gotoPage(pageNumber: number): void {
  updateValue(pageNumber);
}

function decrementPage(): void {
  if (currentPage.value > 1) {
    updateValue(currentPage.value - 1);
  }
}

function incrementPage(): void {
  if (currentPage.value < pageCount.value) {
    updateValue(currentPage.value + 1);
  }
}

function leftEndPage(): void {
  updateValue(1);
}

function rightEndPage(): void {
  updateValue(pageCount.value);
}
function updateValue(value: number) {
  currentPage.value = value;
  emits("click", currentPage.value);
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
