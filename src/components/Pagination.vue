<template>
  <div v-if="pageCount > 1" class="Pagination">
    <div class="PaginationBlock">
      <span
        @click="$emit('page-number', pageNumber > 1 ? pageNumber-1 : pageNumber)"
        class="PaginationItem"
      >&lt;&#8195;</span>

      <span v-if="pageNumber !== 1" @click="$emit('page-number', 1)" class="PaginationItem">1&#8195;</span>

      <span v-if="pageNumber > 3">&#8195;...&#8195;</span>

      <span
        v-if="pageNumber > 2"
        @click="$emit('page-number', pageNumber-1)"
        class="PaginationItem"
      >{{pageNumber-1}}</span>

      <span class="SelectedPage">&#8195;{{pageNumber}}&#8195;</span>

      <span
        v-if="pageCount - pageNumber > 1"
        @click="$emit('page-number', pageNumber+1)"
        class="PaginationItem"
      >{{pageNumber+1}}</span>

      <span v-if="pageCount - pageNumber > 2">&#8195;...&#8195;</span>

      <span
        v-if="pageNumber < pageCount"
        @click="$emit('page-number', pageCount)"
        class="PaginationItem"
      >&#8195;{{pageCount}}</span>

      <span
        @click="$emit('page-number', pageNumber < pageCount ? pageNumber+1 : pageNumber)"
        class="PaginationItem"
      >&#8195;&gt;</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Pagination",
  props: {
    pageNumber: Number,
    pageCount: Number
  }
};
</script>

<style lang="scss" scoped>
.Pagination {
  .PaginationBlock {
    text-align: center;

    .PaginationItem {
      cursor: pointer;
    }

    .SelectedPage {
      cursor: pointer;
      color: red;
    }
  }
}
</style>
