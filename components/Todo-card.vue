<script setup lang="ts">
const { todoObj, deleteFunc, addFunc } = defineProps([
  "todoObj",
  "deleteFunc",
  "addFunc",
]);
const isOpen = ref(false);
const isEditOpen = ref(false);

const logList = () => {
  console.log(todoObj);
};

const handleDeleteOpen = () => {
  isOpen.value = true;
};
const handleDeleteClose = () => {
  isOpen.value = false;
};
const handleDelete = (id) => {
  deleteFunc(id);
  isOpen.value = false;
};

const handleEditOpen = () => {
  isEditOpen.value = true;
};
const handleEditSave = () => {
  isEditOpen.value = false;
  console.log(todoObj);
};

const closeEditModal = () => {
  isEditOpen.value = false;
};

const padZeros = (number, length) => {
  return number.toString().padStart(length, "0");
};

const formatDate = (dateObj) => {
  dateObj = new Date(dateObj);
  const day = padZeros(dateObj.getDate(), 2);
  const month = padZeros(dateObj.getMonth() + 1, 2);
  const year = dateObj.getFullYear();
  const hours = padZeros(dateObj.getHours(), 2);
  const minutes = padZeros(dateObj.getMinutes(), 2);

  return day + "-" + month + "-" + year + " at " + hours + "." + minutes;
};
</script>

<template>
  <div class="todo-card">
    <div class="todo-head">
      <div class="todo-card-title-wrap">
        <h1 class="todo-title">{{ todoObj.title }}</h1>
        <p class="todo-date">Last update: {{ formatDate(todoObj.date) }}</p>
      </div>
      <div>
        <UButton
          icon="i-heroicons-pencil-square"
          size="xl"
          color="primary"
          variant="link"
          :trailing="false"
          @click="handleEditOpen()"
        />
        <EditModal
          v-bind:modelValue="isEditOpen"
          @save="handleEditSave()"
          @close="closeEditModal()"
          :todoListItem="todoObj"
          :addFunc="addFunc"
        />
        <UButton
          icon="i-heroicons-trash"
          size="xl"
          color="red"
          variant="link"
          :trailing="false"
          @click="isOpen = true"
        />
        <DeleteConfirmModal
          v-bind:modelValue="isOpen"
          @delete="handleDelete(todoObj.id)"
          @closeModal="handleDeleteClose()"
        />
      </div>
    </div>
    <ul>
      <li v-for="item in todoObj.list">
        <Todo-item :todo-item="item" :trigger-func="logList" />
      </li>
    </ul>
  </div>
</template>
