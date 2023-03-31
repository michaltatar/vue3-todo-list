<script setup>
import { ref } from 'vue'

const title = ref('')

const emit = defineEmits(['addItem'])

const addItem = () => {
  emit('addItem', title.value)
  title.value = ''
}
</script>

<template>
  <form class="form" @submit.prevent="addItem">
    <div class="form__control">
      <input type="text" v-model="title" name="text" placeholder="Add title…" />
    </div>
    <input type="submit" value="Save" :disabled="!title" />
  </form>
</template>

<style lang="scss" scoped>
.form {
  margin-top: 1.25rem;
  padding: 0.75rem;
  border: 1px solid var(--gray-light-1);
  background-color: var(--color-form-wrapper);

  input {
    width: 100%;

    &[type='submit'] {
      background-color: var(--green-dark);

      &:disabled {
        background-color: var(--gray-light-1);
        cursor: default;
      }
    }
  }

  &__control {
    margin: 1.5rem 0;

    label {
      display: block;
    }
  }
}
</style>
