<script setup>
import { ref, watch } from 'vue';

// generate unique id
let idCounter = 0;
const generateId = () => ++idCounter;

const lists = ref([]);
const keyword = ref('');
const editId = ref(0);

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
  }
};

// Toggle completed status
const toggleCompleted = (item) => {
  item.isCompleted = !item.isCompleted;
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
  <main class="container mx-auto w-fit">
    <form
      @submit.prevent="handleAdd"
      class="flex flex-col items-center gap-2 mt-5"
    >
      <label class="text-center">Todolist</label>
      <div class="flex gap-5">
        <input
          v-model="keyword"
          type="text"
          placeholder="เช่น ซื้อข้าว"
          class="border rounded-full p-2"
        />
        <button type="submit" class="border p-2 rounded-full w-24">Add</button>
      </div>
    </form>

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
            class="border p-2 rounded-full w-24"
          >
            Edit
          </button>
          <button
            @click="handleDelete(item.id)"
            class="border p-2 rounded-full w-24"
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
            class="border p-2 rounded-full w-24"
          >
            Save
          </button>
        </div>
      </li>
    </ol>
  </main>
</template>
