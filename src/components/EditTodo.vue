<script setup>
import { ref } from 'vue'

const props = defineProps({
  itemTitle: String
})

const emit = defineEmits(['editTitle'])

const title = ref(props.itemTitle)

const editTitle = () => {
  emit('editTitle', title.value)
  title.value = ''
}
</script>

<template>
  <form class="form" @submit.prevent="editTitle">
    <div class="form__control">
      <input type="text" v-model="title" name="text" placeholder="Edit title…" />
    </div>
    <input type="submit" value="Save" :disabled="!title || title === itemTitle" />
  </form>
</template>

<style lang="scss" scoped>
.form {
  display: flex;
  flex-basis: 100%;
  margin-top: 0.5rem;
  align-items: center;

  input[type='submit'] {
    margin-left: 1rem;
    background-color: var(--green-dark);

    &:disabled {
      background-color: var(--gray-light-1);
      cursor: default;
    }
  }
}
</style>
