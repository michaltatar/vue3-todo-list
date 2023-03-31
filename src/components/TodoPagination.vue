<script setup>
import { ref } from 'vue'

defineProps({
  paginationItemsCount: Number
})

const emit = defineEmits(['changePagination'])

const activePaginationItem = ref(1)

const changePagination = (index, item) => {
  activePaginationItem.value = item
  emit('changePagination', index)
}
</script>

<template>
  <div class="pagination">
    <p
      v-for="(item, index) in paginationItemsCount"
      :key="item"
      :class="{ 'pagination__item--active': activePaginationItem === item }"
      class="pagination__item"
      @click="changePagination(index, item)"
    >
      {{ item }}
    </p>
  </div>
</template>

<style lang="scss" scoped>
.pagination {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;

  &__item {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0.5rem;
    height: 40px;
    width: 40px;
    border: 1px solid var(--black-soft);
    cursor: pointer;

    &--active {
      background-color: var(--indigo);
      color: var(--white-mute);
    }
  }
}
</style>
