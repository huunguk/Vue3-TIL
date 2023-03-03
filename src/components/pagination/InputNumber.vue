<template>
  <!-- <div>
    <hr />

    <p>
      현재 페이지 번호 : {{ modelValue }} /
      <span>전체 페이지 수 : {{ pageCount }}</span>
    </p>
  </div> -->
  <hr />
  <label>
    원하시는 페이지 번호를 입력하세요 :
    <input type="number" v-model="ChangeNumber" />
    <button @click="onClick(ChangeNumber)">click</button>
  </label>
</template>

<script setup lang="ts">
import { ref, toRefs } from "vue";
const { pageCount, modelValue } = toRefs(props);

const ChangeNumber = ref<number>();

const props = defineProps<{
  modelValue: number;
  pageCount: number;
}>();

const emits = defineEmits<{
  (e: "update:modelValue", value: number): void;
}>();

function onClick(value: number) {
  // input 태그에서 사용자가 입력한 페이지 번호를 currentPage에 반영합니다.
  // 단, 입력한 페이지 번호가 1부터 pageCount까지의 범위에 속하는 경우에만 반영됩니다.
  if (ChangeNumber.value >= 1 && ChangeNumber.value <= props.pageCount) {
    emits("update:modelValue", value);
    ChangeNumber.value = "";
  } else {
    alert("페이지 수를 초과했습니다.");
    ChangeNumber.value = "";
  }
}
</script>

<style scoped>
button {
  margin-left: 5px;
}
</style>
