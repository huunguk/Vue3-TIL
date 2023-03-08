<template>
  <label>
    전체 페이지 수 :
    <input type="number" v-model="PageCountValue" />
    <button @click="onClick(PageCountValue)">click</button>
  </label>
</template>

<script setup lang="ts">
import { ref, toRefs } from "vue";

const PageCountValue = ref<number>();
const { modelValue, pageShowValue } = toRefs(props);

const props = defineProps<{
  modelValue: number;
  pageShowValue: number;
}>();

const emit = defineEmits<{
  (e: "update:modelValue", value: number): void;
}>();

function onClick(value: number) {
  if (PageCountValue.value > pageShowValue.value) {
    emit("update:modelValue", value);
    PageCountValue.value = "";
  } else {
    alert("전체 페이지 수가 페이지네이션 길이보다 짧습니다.");
    PageCountValue.value = "";
  }
}
</script>

<style scoped></style>
