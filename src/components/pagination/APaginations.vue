<template>
  <div>
    <ul class="pagination">
      <button :disabled="currentPage === 1" @click.prevent="leftEndPage">
        ≪
      </button>
      <button :disabled="currentPage === 1" @click.prevent="previousPage">
        ＜
      </button>

      <li
        v-for="page in visiblePages"
        :key="page"
        :class="{ active: currentPage === page }"
      >
        <a
          v-if="page !== '...'"
          href="#"
          @click.prevent="setCurrentPage(page)"
          >{{ page }}</a
        >
        <span v-else>...</span>
      </li>

      <button :disabled="currentPage === pageCount" @click.prevent="nextPage">
        ＞
      </button>
      <button
        :disabled="currentPage === pageCount"
        @click.prevent="rightEndPage"
      >
        ≫
      </button>
    </ul>
    <p style="text-align: center">{{ currentPage }}페이지</p>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";

const currentPage = ref<number>(1); // 현재 페이지
const itemsPerPage = ref<number>(5); // 페이지당 아이템 개수
const items = ref<number[]>([...Array(50).keys()].map((i) => i + 1)); // 전체 아이템 리스트

// 전체 페이지 수를 계산함
const pageCount = computed(() => {
  return Math.ceil(items.value.length / itemsPerPage.value);
});

// 현재 페이지 주변에 보여줄 페이지 수를 설정
const visiblePages = computed(() => {
  const pagesToShow: number = 3;
  const pageDiff: number = Math.floor(pagesToShow / 2);
  let startPage: number = currentPage.value - pageDiff;
  let endPage: number = currentPage.value + pageDiff;
  if (startPage < 1) {
    startPage = 1;
    endPage = pagesToShow;
  }
  if (endPage > pageCount.value) {
    endPage = pageCount.value;
    startPage = endPage - pagesToShow + 1;
  }
  const pages: (number | string)[] = [];
  for (let i: number = startPage; i <= endPage; i++) {
    pages.push(i);
  }
  if (startPage > 1) {
    pages.unshift("...");
    pages.unshift(1);
  }
  if (endPage < pageCount.value) {
    pages.push("...");
    pages.push(pageCount.value);
  }
  return pages;
});

// 현재 페이지에 해당하는 아이템들을 잘라내어 반환
const paginatedItems = computed(() => {
  const startIndex: number = (currentPage.value - 1) * itemsPerPage.value;
  const endIndex: number = startIndex + itemsPerPage.value;
  return items.value.slice(startIndex, endIndex);
});

// 이전, 다음, 첫, 마지막 페이지로 이동하는 함수
function setCurrentPage(page: number): void {
  currentPage.value = page;
}

function previousPage(): void {
  currentPage.value > 1 && currentPage.value--;
}

function nextPage(): void {
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
.pagination {
  display: flex;
  list-style-type: none;
  padding: 0;
  justify-content: center;
  margin-top: 20px;
}

.pagination li {
  width: 50px;
  text-align: center;
  margin: 0 5px;
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
.pagination li a {
  display: block;
  weight: 50px;

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

.items {
  weight: 100%;
  display: flex;
  justify-content: center;
}

.items div {
  width: 130px;
  padding: 15px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  text-align: center;
  margin: 5px;
  border-radius: 10px;
}
</style>
