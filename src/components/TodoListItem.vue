<script setup>
import { ref, computed } from 'vue'
import EditTodo from '../components/EditTodo.vue'

const props = defineProps({
  item: {
    userId: Number,
    id: Number,
    title: String,
    completed: Boolean
  }
})

const emit = defineEmits(['deleteItem', 'editTitle', 'setCompletionStatus'])

const deleteItem = (id) => emit('deleteItem', id)

const editTitle = (title) =>
  emit('editTitle', {
    id: props.item.id,
    title
  })

const setCompletionStatus = (e, id) => {
  const checked = e.target.checked
  emit('setCompletionStatus', {
    checked,
    id
  })
}

const showEditTodoItem = ref(false)

const toggleEditTodoItem = () => {
  showEditTodoItem.value = !showEditTodoItem.value
}

const buttonText = computed(() => (showEditTodoItem.value ? 'Close' : 'Edit'))
</script>

<template>
  <li class="list-item">
    <div class="list-item__input-wrapper">
      <input
        type="checkbox"
        :checked="item.completed"
        @change="(e) => setCompletionStatus(e, item.id)"
      />
      <p
        :class="{ 'list-item__input-wrapper__title--complete': item.completed }"
        class="list-item__input-wrapper__title"
      >
        {{ item.title }}
      </p>
    </div>
    <div class="list-item__button-wrapper">
      <button class="list-item__button--edit" @click="toggleEditTodoItem">{{ buttonText }}</button>
      <button class="list-item__button--delete" @click="deleteItem(item.id)">Delete</button>
    </div>
    <template v-if="showEditTodoItem">
      <edit-todo :itemTitle="item.title" @edit-title="editTitle" />
    </template>
  </li>
</template>

<style lang="scss" scoped>
.list-item {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin: 0.75rem 0;
  padding: 0.75rem;
  background-color: var(--color-list-item);

  &__input-wrapper {
    display: flex;

    &__title {
      margin-left: 0.5rem;
      max-width: 400px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;

      &--complete {
        text-decoration: line-through;
      }
    }
  }

  &__button-wrapper {
    display: flex;
  }

  &__button--edit {
    margin: 0 0.5rem 0 0.5rem;
    background-color: var(--blue-light);
  }

  &__button--delete {
    background-color: var(--red-light);
  }
}
</style>
