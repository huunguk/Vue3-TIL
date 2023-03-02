<template>
  <div class="pagination">
    <button @click="leftEndPage" :disabled="currentPage === 1">≪</button>
    <button @click="incrementPage" :disabled="currentPage === 1">＜</button>
    <ul>
      <li
        v-for="(pageNumber, index) in pageCount"
        :key="pageNumber"
        :class="{ active: pageNumber === currentPage }"
      >
        <a href="#" @click.prevent="gotoPage(pageNumber)">
          {{ pageNumber }}
        </a>
      </li>
      <span>...</span>
    </ul>
    <button @click="decrementPage" :disabled="currentPage === pageCount">
      ＞
    </button>
    <button @click="rightEndPage" :disabled="currentPage === pageCount">
      ≫
    </button>
  </div>
  <!-- pageCount, currentPage를 props로 전달 -->
  <InputNumber :pageCount="pageCount" v-model="currentPage"></InputNumber>
</template>

<script setup lang="ts">
import InputNumber from "./InputNumber.vue";
import { ref, computed } from "vue";

const props = withDefault(
  defineProps<{ total: number; showLength?: number; modelValue?: number }>(),
  {
    showLength: 10,
    modelValue: 0,
  }
);

const currentPage = ref<number>(); //현재 페이지 번호 추적
// const pageSize: number = 10; //페이지당 아이템 수
// const totalItems: number = 200; //전체 아이템 수
// const pageCount: number = Math.ceil(totalItems / pageSize); //전체 페이지 수를 계산하는데 사용
const pageCount: number = 20; //전체 페이지 수를 계산하는데 사용

const pages = computed(() => {
  if (props.showLength <= props.total) {
    return; //[1, props.showLength]
  }
  //1, props.total
});
onMounted(() => {
  currentPage.value = props.modelValue;
});
watch(toRef("modelValue"), () => {
  currentPage.value = props.modelValue;
});

function gotoPage(pageNumber: number) {
  currentPage.value = pageNumber;
  // 페이지 번호를 변경하면 필요한 작업을 수행.
  // ex.API를 호출하여 새로운 데이터를 가져올 수 있습니다.
}

function decrementPage(): void {
  currentPage.value < pageCount && currentPage.value++;
}
function incrementPage(): void {
  currentPage.value > 1 && currentPage.value--;
}
function leftEndPage(): void {
  currentPage.value = 1;
}
function rightEndPage(): void {
  currentPage.value = pageCount;
}
</script>

<style scoped>
* {
  list-style: none;
  padding: 0;
}
.pagination {
  display: flex;
  align-items: center;
}

.pagination button {
  width: 30px;
  height: 30px;
  margin: 5px;
}

.pagination ul {
  display: flex;
  align-items: center;
  justify-content: center;
}
.pagination li {
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.pagination li.active a {
  font-weight: bold;
  color: red;
}
</style>
