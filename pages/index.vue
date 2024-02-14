<script setup lang="ts">
import type { FormError, FormSubmitEvent } from "#ui/types";

const formState = reactive({
  userName: undefined,
});

const validate = (formState: any): FormError[] => {
  const errors = [];
  if (!formState.userName)
    errors.push({ path: "userName", message: "Required" });
  return errors;
};

const checkUser = (async () => {
  // let userName = localStorage.getItem("todo-userName");
  if (userName) {
    await navigateTo("/todo");
  }
})();

async function onSubmit(event: FormSubmitEvent<any>) {
  localStorage.setItem("todo-userName", formState.userName);
  console.log(formState.userName);
  await navigateTo("/todo");
}
</script>

<template>
  <div class="starter-page">
    <div class="starter-inner">
      <h1>
        Welcome to the ✔️<span class="brand"><strong>to-do</strong></span> app
        you might want to use in an emergency🤯
      </h1>
      <p>What should we call you?</p>

      <UForm
        :validate="validate"
        :state="formState"
        @submit="onSubmit"
        class="starter-form"
      >
        <UFormGroup name="userName">
          <UInput
            v-model="formState.userName"
            type="text"
            class="starter-input"
            size="xl"
            placeholder="Your name..."
            variant="none"
          />
        </UFormGroup>

        <UButton class="starter-btn mt-5" type="submit" size="xl" color="white">
          Let's Start
        </UButton>
      </UForm>
    </div>
  </div>
</template>
