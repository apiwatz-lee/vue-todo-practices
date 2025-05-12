<script setup>
import { computed } from 'vue';

const props = defineProps({
  placeholder: String,
  modelValue: {
    type: String,
    required: true,
  },
});

const emit = defineEmits(['update:modelValue']);

const modelValue = computed({
  get() {
    return props.modelValue;
  },

  set(newValue) {
    emit('update:modelValue', newValue);
  },
});
</script>

<template>
  <input
    class="border rounded-full p-2"
    type="text"
    :placeholder="placeholder"
    v-model="modelValue"
  />
</template>
