<template>
  <ul class="pagination">
    <li v-for="({ value, type }, index) in range" :key="index">
        <div
          v-if="type === 'page'"
          class="pagination__page"
          :class="{'pagination__page--selected': value === dataCurrentPage}"
          @click="updatePage(value)">{{ value }}
        </div>
        <span v-else>{{ value }}</span>
    </li>
  </ul>
</template>

<script lang="ts">
import { defineComponent, computed, ref, watch } from 'vue'

export default defineComponent({
  name: 'Pagination',
  props: {
    pageCount: {
      type: Number,
      required: true,
      validator: (n: number) => n > 0
    },
    currentPage: {
      type: Number,
      default: 1,
      validator: (n: number) => n > 0
    }
  },
  setup (props, { emit }) {
    const dataCurrentPage = ref<number>(props.currentPage)
    const pageCount = ref<number>(props.pageCount)
    const range = computed(() => {
      const fullPageArray = new Array(pageCount.value).fill(1).map((item, index) => ({ value: item + index, type: 'page' }))
      const currentPageRange = [dataCurrentPage.value - 1, dataCurrentPage.value, dataCurrentPage.value + 1].map((item) => ({ value: item, type: 'page' }))
      const startPage = fullPageArray.slice(0, 1)[0]
      const lastPage = fullPageArray.slice(-1)[0]
      const separator = { value: '...', type: 'separator' }

      if (pageCount.value <= 5) return fullPageArray
      if (pageCount.value > 5) {
        if (dataCurrentPage.value === startPage.value || dataCurrentPage.value === startPage.value + 1) return [...fullPageArray.slice(0, 4), separator, lastPage]

        if (dataCurrentPage.value === startPage.value + 2) return [startPage, ...currentPageRange, separator, lastPage]

        if (dataCurrentPage.value === startPage.value - 2) return [startPage, separator, ...currentPageRange, lastPage]

        if (dataCurrentPage.value === lastPage.value || dataCurrentPage.value === lastPage.value - 1) return [startPage, separator, ...fullPageArray.slice(-4)]
      }
      return [startPage, separator, ...currentPageRange, separator, lastPage]
    })

    watch(() => dataCurrentPage.value, (val) => {
      dataCurrentPage.value = val
    })

    const updatePage = (page: number) => {
      dataCurrentPage.value = page
      emit('select', page)
    }
    return {
      dataCurrentPage, pageCount, range, updatePage
    }
  }
})
</script>

<style lang="scss">
.pagination {
  display: flex;
  align-items: center;
  justify-content: flex-start;

  & > :not(:last-child) {
    margin-right: 24px;
  }
}

.pagination__page {
  font-size: $fontSize14;
  cursor: pointer;
}

.pagination__page--selected {
  font-weight: 600;
}
</style>
