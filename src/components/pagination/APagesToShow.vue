<template>
  <label>
    페이지네이션 길이 :
    <input type="number" v-model="InputPageValue" />
    <button @click="onClick(InputPageValue)">click</button>
  </label>
</template>

<script setup lang="ts">
import { ref, toRefs } from "vue";

const InputPageValue = ref<number>();
const { modelValue, pageCountValue } = toRefs(props);

const props = defineProps<{
  modelValue: number;
  pageCountValue: number;
}>();

const emits = defineEmits<{
  (e: "update:modelValue", value: number): void;
}>();

function onClick(value: number) {
  if (InputPageValue.value <= pageCountValue.value) {
    emits("update:modelValue", value);
    InputPageValue.value = "";
  } else {
    alert("페이지네이션 길이가 전체 페이지 수 보다 깁니다.");
    InputPageValue.value = "";
  }
}
</script>

<style scoped></style>
