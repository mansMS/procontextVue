<template>
  <div v-if="pageCount > 1" class="Pagination">
    <span
      @click="$emit('go-to-page', pageNumber > 1 ? pageNumber-1 : pageNumber)"
      class="PaginationItem"
    >&lt;&#8195;</span>

    <span v-if="pageNumber !== 1" @click="$emit('go-to-page', 1)" class="PaginationItem">1&#8195;</span>

    <span v-if="pageNumber > 3">&#8195;...&#8195;</span>

    <span
      v-if="pageNumber > 2"
      @click="$emit('go-to-page', pageNumber-1)"
      class="PaginationItem"
    >{{pageNumber-1}}</span>

    <span class="SelectedPage">&#8195;{{pageNumber}}&#8195;</span>

    <span
      v-if="pageCount - pageNumber > 1"
      @click="$emit('go-to-page', pageNumber+1)"
      class="PaginationItem"
    >{{pageNumber+1}}</span>

    <span v-if="pageCount - pageNumber > 2">&#8195;...&#8195;</span>

    <span
      v-if="pageNumber < pageCount"
      @click="$emit('go-to-page', pageCount)"
      class="PaginationItem"
    >&#8195;{{pageCount}}</span>

    <span
      @click="$emit('go-to-page', pageNumber < pageCount ? pageNumber+1 : pageNumber)"
      class="PaginationItem"
    >&#8195;&gt;</span>
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
  .PaginationItem {
    cursor: pointer;
  }

  .SelectedPage {
    cursor: pointer;
    color: red;
  }
}
</style>
