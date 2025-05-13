<script setup>
const props = defineProps({
  label: String,
});

const emit = defineEmits({});
</script>

<template>
  <button
    @click="handleEdit(item.id)"
    class="border p-2 rounded-full w-24 cursor-pointer"
  >
    Edit
  </button>
</template>
