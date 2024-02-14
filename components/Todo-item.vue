<script setup lang="ts">
const { todoItem, triggerFunc } = defineProps(["todoItem", "trigger-func"]);
</script>

<template>
  <div class="todo-item" v-if="todoItem">
    <UCheckbox
      v-model="todoItem.status"
      :name="todoItem.label"
      @change="triggerFunc"
    />
    <p v-if="!todoItem.status">{{ todoItem.label }}</p>
    <p v-if="todoItem.status" class="strike">{{ todoItem.label }}</p>
  </div>
</template>
