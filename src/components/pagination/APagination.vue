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
// const pageCount = ref<number>(20); //전체 페이지 수
// const pagesToShow = ref<number>(8); //페이지네이션 길이

const props = withDefaults(
  defineProps<{
    modelValue: number; //현재 페이지 번호를 받아올 prop
    pagesToShow?: number;
    pageCount: number;
  }>(),
  { pageShowValue: 11 }
);

const { modelValue, pagesToShow, pageCount } = toRefs(props);

const emits = defineEmits<{
  (e: "update:currentPage", value: number): void; // 현재 페이지 번호 업데이트 emit
  (e: "update:pagesToShow", value: number): void;
  (e: "update:pageCount", value: number): void;
  (e: "click", value: number): void;
}>();

const visiblePage = computed(() => {
  const pages: string[] = []; // 반환할 페이지들을 담을 배열
  const pageToShowHalf: number = Math.floor(pagesToShow.value / 2); // 페이지네이션 길이의 반 // 4
  let startPage: number = currentPage.value - pageToShowHalf; // 시작 페이지 // ? - 4
  let endPage: number = currentPage.value + pageToShowHalf; // 끝 페이지 // ? + 4
  let numberAverage: number = Math.ceil(pagesToShow.value / 2); // 페이지네이션 길이의 반을 올림 // 4

  // console.log("pages.length", pages.length);

  // if (currentPage.value <= pagesToShow.value - 2) {
  //   for(let i=0;i<pagesToShow.value - 2; i++) {
  //     pages.push(i+1)
  //   }
  //   pages.push('...', pageCount.value.toString())
  // } else if() {

  // }

  // 시작 페이지가 1 이하일 때
  if (startPage <= numberAverage) {
    startPage = 1;
    endPage = pagesToShow.value;
  }
  // 끝 페이지가 전체 페이지 수보다 크거나 같을 때
  else if (endPage > pageCount.value - numberAverage) {
    endPage = pageCount.value;
    startPage = endPage - pagesToShow.value + 1;
  }
  // 시작 페이지부터 끝 페이지까지 반복하면서 페이지들을 배열에 push
  for (let i: number = startPage; i <= endPage; i++) {
    pages.push(i);
  }

  // 시작 페이지가 1보다 크면
  if (startPage > 1) {
    pages.unshift("...");
    pages.unshift(1);
    pages.pop();
  }

  if (startPage > 1 && endPage < pageCount.value) {
    if (pagesToShow.value % 2 === 1) {
      pages.splice(2, 1);
    } else {
      pages.splice(2, 2);
    }
  }

  if (endPage < pageCount.value) {
    // 끝 페이지가 전체 페이지 수보다 작으면
    pages.push("...");
    pages.push(pageCount.value);
  } else if (endPage >= pageCount.value) {
    pages.push(pageCount.value);
  }
  console.log("pages.length", pages.length);

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

// watch(pagesToShow, () => {
//   pagesToShow.value = pageShowValue.value;
// });
// watch(pageCountValue, () => {
//   pageCount.value = pageCountValue.value;
// });

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
    // currentPage.value++;
    updateValue(currentPage.value + 1);
  }
}

function leftEndPage(): void {
  // currentPage.value = 1;
  updateValue(1);
}

function rightEndPage(): void {
  updateValue(pageCount.value);
  // currentPage.value = pageCount.value;
  // emits("click", currentPage.value);
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
