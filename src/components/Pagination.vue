<template>
  <nav class="mt-3">
    <ul class="pagination d-flex flex-wrap" v-if="pages > 1">
      <li class="page-item">
        <a
          class="page-link"
          href="javascript:;"
          aria-label="Previous"
          @click="handleClick('dec')"
        >
          <span aria-hidden="true">&laquo;</span>
        </a>
      </li>
      <li
        class="page-item"
        v-for="page in pages"
        :key="page"
        :class="{ active: page - 1 === currentPage }"
      >
        <a class="page-link" href="javascript:;" @click="handleClick(page - 1)">{{
          page
        }}</a>
      </li>
      <li class="page-item">
        <a
          class="page-link"
          href="javascript:;"
          aria-label="Next"
          @click="handleClick('inc')"
        >
          <span aria-hidden="true">&raquo;</span>
        </a>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  props: {
    pages: {
      type: Number,
      required: true
    },
    currentPage: {
      type: Number,
      required: true
    }
  },
  methods: {
    handleClick(val) {
      this.$emit('handlePageChange', val)
    }
  }
}
</script>

<style></style>
