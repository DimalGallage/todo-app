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
</script>

<template>
  <div class="todo-card">
    <div class="todo-head">
      <div class="todo-card-title-wrap">
        <h1 class="todo-title">{{ todoObj.title }}</h1>
        <p class="todo-date">Last update: {{ todoObj.date }}</p>
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
