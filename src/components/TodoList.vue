<script setup>
import InputText from './InputText.vue';
defineProps({
  lists: Array,
  editId: Number,
});
const emit = defineEmits([
  'toggleCompleted',
  'handleEdit',
  'handleDelete',
  'handleSave',
]);

const toggleCompleted = (item) => {
  emit('toggleCompleted', item);
};

const handleEdit = (id) => {
  emit('handleEdit', id);
};

const handleDelete = (id) => {
  emit('handleDelete', id);
};

const handleSave = (id, keyword) => {
  emit('handleSave', id, keyword);
};
</script>

<template>
  <ol class="flex flex-col justify-start items-start gap-2 mt-5">
    <li v-for="item in lists" :key="item.id">
      <div v-if="item.id !== editId" class="flex gap-2">
        <input
          type="checkbox"
          :checked="item.isCompleted"
          @change="toggleCompleted(item)"
        />
        <span
          class="flex justify-center items-center w-fit"
          :class="[item.isCompleted && 'line-through text-gray-400']"
        >
          {{ item.keyword }}
        </span>
        <button
          @click="handleEdit(item.id)"
          class="border p-2 rounded-full w-24 cursor-pointer"
        >
          Edit
        </button>
        <button
          @click="handleDelete(item.id)"
          class="border p-2 rounded-full w-24 cursor-pointer"
        >
          Delete
        </button>
      </div>

      <div v-else class="flex gap-2">
        <input
          type="text"
          v-model="item.keyword"
          class="border p-2 rounded-full"
          @keyup.enter="handleSave(item.id, item.keyword)"
        />
        <button
          @click="handleSave(item.id, item.keyword)"
          class="border p-2 rounded-full w-24 cursor-pointer"
        >
          Save
        </button>
      </div>
    </li>
  </ol>
</template>
