<template>
  <div>
    <p>
      현재 페이지 번호 : {{ currentPage }} /
      <span>전체 페이지 수 : {{ pageCount }}</span>
    </p>
  </div>
  <hr />
  <label>
    원하시는 페이지 번호를 입력하세요 :
    <input type="number" v-model="inputPage" />
    <button @click="$emit('change-number')">click</button>
  </label>
</template>

<script setup lang="ts">
import { defineProps, defineEmits, ref } from "vue";

const props = defineProps({
  currentPage: Number,
  pageCount: Number,
});

const inputPage = ref<number>(props.currentPage);

function gotoPage() {
  // input 태그에서 사용자가 입력한 페이지 번호를 currentPage에 반영합니다.
  // 단, 입력한 페이지 번호가 1부터 pageCount까지의 범위에 속하는 경우에만 반영됩니다.
  if (inputPage.value >= 1 && inputPage.value <= props.pageCount) {
    props.currentPage = inputPage.value;
  }
}

defineEmits(["change-number"]);
</script>

<style scoped>
/* .container {
}

.container p {
  width: 150px;
  font-weight: 900;
  margin: 30px;
}

.container input {
  width: 100px;
  height: 20px;
  margin: 30px;
} */
</style>
