<template>
  <hr />
  <label>
    원하시는 페이지 번호를 입력하세요 :
    <input type="number" v-model="ChangeNumber" />
    <button @click="onClick(ChangeNumber)">click</button>
  </label>
</template>

<script setup lang="ts">
import { ref, toRef } from "vue";
const { modelValue } = toRef(props);

const props = defineProps<{
  modelValue: number;
}>();

const ChangeNumber = ref<number>(1);

const emits = defineEmits<{
  (e: "update:modelValue", value: number): void;
}>();

function onClick(value: number) {
  // input 태그에서 사용자가 입력한 페이지 번호를 currentPage에 반영.
  // 단, 입력한 페이지 번호가 1부터 pageCount까지의 범위에 속하는 경우에만 반영.
  if (ChangeNumber.value >= 1 || ChangeNumber.value <= 20) {
    emits("update:modelValue", value);
    ChangeNumber.value = "";
  } else {
    alert("no");
    ChangeNumber.value = "";
  }
}
</script>

<style scoped>
button {
  margin-left: 5px;
}
</style>
