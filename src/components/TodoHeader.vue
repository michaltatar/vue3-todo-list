<script setup>
import { ref, computed } from 'vue'
import AddTodo from '../components/AddTodo.vue'

defineProps({
  buttonText: String
})

const emit = defineEmits(['addTodoItem'])

const addTodoItem = (title) => {
  emit('addTodoItem', title)
}

const showAddTodoItem = ref(false)

const toggleAddTodoItem = () => {
  showAddTodoItem.value = !showAddTodoItem.value
}

const buttonText = computed(() => (showAddTodoItem.value ? 'Close' : 'Add'))

const darkMode = ref(false)

const setDarkMode = () => {
  document.querySelector('body').classList.add('dark-mode')
  darkMode.value = true
}

const setLightMode = () => {
  document.querySelector('body').classList.remove('dark-mode')
  darkMode.value = false
}

const toggleDarkMode = () => {
  if (darkMode.value || document.querySelector('body').classList.contains('dark-mode')) {
    setLightMode()
  } else {
    setDarkMode()
  }
}
</script>

<template>
  <div class="header">
    <label class="header__switch">
      <input type="checkbox" @change="toggleDarkMode" />
      <span class="header__switch-slider"></span>
    </label>
    <h1>Todo List</h1>
    <button @click="toggleAddTodoItem" class="header__button--add">{{ buttonText }}</button>
  </div>
  <div v-show="showAddTodoItem">
    <add-todo @add-item="addTodoItem" />
  </div>
</template>

<style lang="scss" scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;

  &__switch {
    position: relative;
    display: inline-block;
    width: 60px;
    height: 34px;

    input {
      opacity: 0;
      width: 0;
      height: 0;
    }

    &-slider {
      position: absolute;
      cursor: pointer;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: var(--gray);
      -webkit-transition: 0.4s;
      transition: 0.4s;
      border-radius: 34px;

      &:before {
        position: absolute;
        content: '';
        height: 26px;
        width: 26px;
        left: 4px;
        bottom: 4px;
        background-color: var(--white);
        -webkit-transition: 0.4s;
        transition: 0.4s;
        border-radius: 50%;
      }
    }

    input:checked + &-slider {
      background-color: var(--blue-darker);
    }

    input:focus + &-slider {
      box-shadow: 0 0 1px var(--blue-darker);
    }

    input:checked + &-slider:before {
      -webkit-transform: translateX(26px);
      -ms-transform: translateX(26px);
      transform: translateX(26px);
    }
  }

  &__button--add {
    background-color: var(--indigo-soft);
  }
}
</style>
