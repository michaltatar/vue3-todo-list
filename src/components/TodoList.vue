<script setup>
import { computed, ref } from 'vue'
import TodoHeader from '../components/TodoHeader.vue'
import TodoListItem from '../components/TodoListItem.vue'
import TodoPagination from '../components/TodoPagination.vue'

const failedToFetchData = ref(false)

const isFetchingData = ref(true)

const todoList = ref([])

const responseTotalCount = ref(0)

const ITEMS_TO_DISPLAY_LIMIT = 20

const getTodos = async (start, limit) => {
  try {
    const url = `https://jsonplaceholder.typicode.com/todos?_start=${start}&_limit=${limit}`
    const res = await fetch(url)
    responseTotalCount.value = res.headers.get('x-total-count')
    const data = await res.json()
    todoList.value = data
  } catch (err) {
    failedToFetchData.value = true
    console.error(err)
  } finally {
    isFetchingData.value = false
  }
}

const addTodoItem = (title) => {
  const newTodo = {
    userId: 1,
    id: todoList.value[todoList.value.length - 1].id + 1,
    title,
    completed: false
  }

  todoList.value.push(newTodo)
}

const deleteTodoItem = (id) => {
  todoList.value = todoList.value.filter((item) => item.id !== id)
}

const updateData = (propName, id, value) => {
  const index = todoList.value.findIndex((item) => {
    return item.id === id
  })

  if (index !== -1) {
    todoList.value[index][propName] = value
  }
}

const editTitle = (payload) => {
  const { title, id } = payload

  updateData('title', id, title)
}

const setCompletionStatus = (payload) => {
  const { checked, id } = payload

  updateData('completed', id, checked)
}

// get initial data
getTodos(0, ITEMS_TO_DISPLAY_LIMIT)

const searchInput = ref('')

const filteredTodos = computed(() =>
  todoList.value.filter((item) =>
    item.title.toLowerCase().includes(searchInput.value.toLowerCase())
  )
)

const paginationItemsCount = computed(() =>
  Math.ceil(responseTotalCount.value / ITEMS_TO_DISPLAY_LIMIT)
)

const changePagination = (index) => {
  let start = 0
  start ||= index * ITEMS_TO_DISPLAY_LIMIT
  getTodos(start, ITEMS_TO_DISPLAY_LIMIT)
}
</script>

<template>
  <todo-header @add-todo-item="addTodoItem" />
  <template v-if="failedToFetchData"><p>Failed to fetch data</p></template>
  <template v-else>
    <div class="todos__container">
      <template v-if="isFetchingData">Fetching data...</template>
      <template v-else>
        <input type="text" v-model.lazy="searchInput" placeholder="Search todo…" />
        <ul v-if="todoList.length" class="todos__container-list">
          <todo-list-item
            v-for="item in filteredTodos"
            :key="item.id"
            :item="item"
            @delete-item="deleteTodoItem"
            @edit-title="editTitle"
            @set-completion-status="setCompletionStatus"
          />
        </ul>
      </template>
    </div>
    <todo-pagination
      :pagination-items-count="paginationItemsCount"
      @change-pagination="changePagination"
    />
  </template>
</template>

<style lang="scss" scoped>
.todos__container {
  margin-top: 1.5rem;

  &-list {
    margin-top: 1rem;
    padding: 0;
    list-style-type: none;
  }
}
</style>
