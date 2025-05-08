<script setup>
import { ref, watch } from 'vue';

// generate unique id
let idCounter = 0;
const generateId = () => ++idCounter;

const lists = ref([]);
const keyword = ref('');

// Add new item
const handleAdd = () => {
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
  lists.value.forEach((item) => {
    item.isEdit = item.id === id;
  });
};

// Save edited item
const handleSave = (id, newKeyword) => {
  const trimmed = newKeyword.trim();
  if (!trimmed) return;

  const item = lists.value.find((i) => i.id === id);
  if (item) {
    item.keyword = trimmed;
    item.isEdit = false;
  }
};

// Toggle completed status
const toggleCompleted = (id) => {
  const item = lists.value.find((i) => i.id === id);
  if (item) {
    item.isCompleted = !item.isCompleted;
  }
};

watch(
  lists,
  (newVal) => {
    console.log('--- new ----', newVal);
  },
  { deep: true }
);
</script>

<template>
  <form @submit.prevent="handleAdd" class="form">
    <label>Todolist</label>
    <div>
      <input
        v-model="keyword"
        type="text"
        placeholder="เช่น ซื้อข้าว"
        class="input"
      />
      <button type="submit" style="margin-left: 8px">Add</button>
    </div>
  </form>

  <ol class="list-container">
    <li v-for="item in lists" :key="item.id" class="list-wrapper">
      <div v-if="!item.isEdit" class="list-content">
        <input
          type="checkbox"
          :checked="item.isCompleted"
          @change="toggleCompleted(item.id)"
        />
        <span :class="{ completed: item.isCompleted }">
          {{ item.keyword }}
        </span>
        <button @click="handleEdit(item.id)">Edit</button>
        <button @click="handleDelete(item.id)">Delete</button>
      </div>

      <div v-else class="list-content">
        <input
          type="text"
          class="input"
          v-model="item.keyword"
          @keyup.enter="handleSave(item.id, item.keyword)"
        />
        <button @click="handleSave(item.id, item.keyword)">Save</button>
      </div>
    </li>
  </ol>
</template>

<style scoped>
.input {
  border-radius: 24px;
  height: 24px;
  padding: 8px;
}

.form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 8px;
}

.list-container {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.list-content {
  display: flex;
  justify-content: start;
  align-items: center;
}

.completed {
  text-decoration: line-through;
  color: gray;
}

button {
  margin-left: 8px;
}
</style>
