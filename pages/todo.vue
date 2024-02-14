<script setup lang="ts">
const todoList = ref([]);
const name = ref("");
const newListModal = ref(false);
const newListObjId = ref();
const newListNewCheck = ref("");
const newListObj = ref({
  id: newListObjId,
  title: "",
  date: "",
  list: [],
});

// Function to delete a list
const deleteList = (id) => {
  const indexToRemove = todoList.value.findIndex((item) => item.id === id);
  if (indexToRemove !== -1) {
    todoList.value.splice(indexToRemove, 1);
  }
  // console.log(todoList);
  saveData();
};

// Function to add a checkbox item to existing list from modal
const addCheckitem = (id, labelTxt) => {
  const indexToEdit = todoList.value.findIndex((item) => item.id === id);
  todoList.value[indexToEdit].list.push({ label: labelTxt, status: false });
  todoList.value[indexToEdit].date = Date.now();
  saveData();
};

// Function to add a new checkbox to new list
const addNewListcheckItem = (id, labelTxt) => {
  newListObj.value.list.push({ label: labelTxt, status: false });
};

//function to create new id
const getNewId = () => {
  if (todoList.value) {
    let newId;
    do {
      newId = Math.round(Math.random().toFixed(3) * 100);
    } while (todoList.value.some((obj) => obj.id === newId));
    newListObjId.value = newId;
    return newId;
  } else {
    newListObjId.value = 1;
    return 1;
  }
};

//Function to open new list modal
const openNewListModal = () => {
  newListObjId.value = getNewId();
  newListModal.value = true;
};

// Function to add a new list
const addNewlist = (newTodoList) => {
  if (todoList.value.length != 0) {
    todoList.value.push({
      id: newListObjId.value,
      title: newTodoList.title,
      date: Date.now(),
      list: newTodoList.list,
    });
  } else {
    console.log("new list");
    todoList.value.push({
      id: 1,
      title: newTodoList.title,
      date: Date.now(),
      list: newTodoList.list,
    });
  }
  newListModal.value = false;
  newListObj.value = {
    title: "",
    date: "",
    list: [],
  };

  saveData();
};

//Save Data to localstorage
const saveData = () => {
  localStorage.setItem("todoAppData", JSON.stringify(todoList.value));
};

onMounted(async () => {
  if (
    JSON.parse(localStorage.getItem("todoAppData")) != null ||
    JSON.parse(localStorage.getItem("todoAppData")) != undefined
  ) {
    todoList.value = JSON.parse(localStorage.getItem("todoAppData"));
    name.value = localStorage.getItem("todo-userName");
  } else {
    localStorage.setItem("todoAppData", JSON.stringify(todoList.value));
    todoList.value = [];
    name.value = localStorage.getItem("todo-userName");
  }
});
</script>

<template>
  <div class="floatingBtnWrap">
    <div class="floatingBtnWrapBar">
      <UButton
        size="xl"
        color="primary"
        @click="openNewListModal()"
        v-if="todoList.length != 0"
        class="floatingBtn"
        >Add New List
      </UButton>
      <EditModal
        v-bind:modelValue="newListModal"
        @save="addNewlist(newListObj)"
        :todoListItem="newListObj"
        :addFunc="addNewListcheckItem"
      />
    </div>
    <p>
      Hello! 👋 <strong>{{ name }}</strong>
    </p>
  </div>

  <div class="todo-card-wrapper" v-if="todoList.length != 0">
    <Todo-card
      v-for="card in todoList"
      :todoObj="card"
      :deleteFunc="deleteList"
      :addFunc="addCheckitem"
    ></Todo-card>
  </div>

  <div class="startOverMsg" v-if="todoList.length == 0">
    <h1>Looks like you haven't created a list yet 🤔</h1>
    <UButton size="xl" color="primary" @click="openNewListModal()">
      Create a New List 😎
    </UButton>
  </div>
</template>
