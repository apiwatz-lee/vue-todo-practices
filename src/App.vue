<script setup>
import { ref, watch } from 'vue';
import TodoForm from './components/TodoForm.vue';
import TodoList from './components/TodoList.vue';

// generate unique id
let idCounter = 0;
const generateId = () => ++idCounter;
const lists = ref([]);
const editId = ref(0);

// Add new item
const handleAdd = (keyword) => {
  const trimmed = keyword.value.trim();
  if (!trimmed) return;

  lists.value.push({
    id: generateId(),
    keyword: trimmed,
    isEdit: false,
    isCompleted: false,
  });

  keyword.value = '';
};

// Delete item by id
const handleDelete = (id) => {
  lists.value = lists.value.filter((item) => item.id !== id);
};

// Enter edit mode for specific item
const handleEdit = (id) => {
  editId.value = id;
};

// Save edited item
const handleSave = (id, newKeyword) => {
  const trimmed = newKeyword.trim();
  if (!trimmed) return;
  const item = lists.value.find((i) => i.id === id);
  if (item) {
    item.keyword = trimmed;
    item.isEdit = false;
    editId.value = 0;
  }
};

// Toggle completed status
const toggleCompleted = (item) => {
  item.isCompleted = !item.isCompleted;
};

// watch(
//   lists,
//   (newVal) => {
//     console.log('--- new ----', newVal);
//   },
//   { deep: true }
// );
</script>

<template>
  <main class="container mx-auto w-fit">
    <TodoForm @add="handleAdd" />
    <TodoList
      :lists="lists"
      :editId="editId"
      @toggleCompleted="toggleCompleted"
      @handleEdit="handleEdit"
      @handleDelete="handleDelete"
      @handleSave="handleSave"
    />
  </main>
</template>
