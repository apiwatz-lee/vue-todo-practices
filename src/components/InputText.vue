<script setup>
const props = defineProps({
  placeholder: String,
});

const keyword = defineModel('keyword');
</script>

<template>
  <input
    class="border rounded-full p-2"
    type="text"
    :placeholder="placeholder"
    v-model="keyword"
  />
</template>
