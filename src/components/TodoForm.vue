<script setup>
import { ref } from 'vue';

const emit = defineEmits(['add']);
const keyword = ref('');

const onAdd = () => {
  emit('add', keyword);
  keyword.value = '';
};
</script>

<template>
  <form
    @submit.prevent="onAdd()"
    class="flex flex$-col items-center gap-2 mt-5"
  >
    <label class="text-center">Todolist</label>
    <div class="flex gap-5">
      <input
        v-model="keyword"
        type="text"
        placeholder="เช่น ซื้อข้าว"
        class="border rounded-full p-2"
      />
      <button type="submit" class="border p-2 rounded-full w-24 cursor-pointer">
        Add
      </button>
    </div>
  </form>
</template>
