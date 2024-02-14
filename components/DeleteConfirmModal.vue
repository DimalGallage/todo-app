<script setup lang="ts"></script>

<template>
  <UModal v-model="modelValue">
    <div class="confirmWrap p-8">
      <p>Are you sure?</p>
      <div class="confirmBtnWrap">
        <UButton color="red" variant="solid" @click="$emit('delete')" size="xl"
          >Yes, Delete</UButton
        >
        <UButton
          color="primary"
          variant="outline"
          @click="$emit('closeModal')"
          size="xl"
          >No, Keep List</UButton
        >
      </div>
    </div>
  </UModal>
</template>
