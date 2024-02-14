<script setup lang="ts">
const props = defineProps(["todoListItem", "modelValue", "addFunc"]);
const newCheckItem = reactive({
  label: "",
});

const addNewItem = () => {
  props.addFunc(props.todoListItem.id, newCheckItem.label);
  newCheckItem.label = "";
};
</script>

<template>
  <UModal v-model="props.modelValue">
    <div class="editModal">
      <div>
        <UForm>
          <div class="titleInput">
            <UFormGroup name="listTitle" label="Title">
              <UInput
                v-model="todoListItem.title"
                type="text"
                size="xl"
                :model-value="todoListItem.title"
              />
            </UFormGroup>
          </div>

          <div class="checkInput">
            <p class="m-1">Enter a checklist item below and click "Add"</p>
            <div v-for="listItem in todoListItem.list" class="checkInputInner">
              <UCheckbox v-model="listItem.status" class="checkInputCheckBox" />
              <UInput
                v-model="listItem.label"
                type="text"
                size="xl"
                :model-value="listItem.label"
                class="checkInputLabel"
              />
            </div>
          </div>

          <div class="addCheckBtn">
            <div class="addCheckBtnInner">
              <UInput
                type="text"
                size="xl"
                placeholder="Add a new Item"
                v-model="newCheckItem.label"
                class="addCheckBtnInput"
              />
              <UButton size="xl" color="primary" @click="addNewItem()">
                Add
              </UButton>
            </div>
          </div>

          <div class="saveBtn">
            <UButton size="xl" color="primary" @click="$emit('save')">
              Save
            </UButton>
          </div>
        </UForm>
      </div>
    </div>
  </UModal>
</template>
