<template>
  <div v-if="pageCount > 1" class="Pagination">
    <span
      @click="$emit('update:page-number', pageNumber > 1 ? pageNumber-1 : pageNumber)"
      class="Pagination-Item"
    >&lt;&#8195;</span>

    <span
      v-if="pageNumber !== 1"
      @click="$emit('update:page-number', 1)"
      class="Pagination-Item"
    >1&#8195;</span>

    <span v-if="pageNumber > 3">&#8195;...&#8195;</span>

    <span
      v-if="pageNumber > 2"
      @click="$emit('update:page-number', pageNumber-1)"
      class="Pagination-Item"
    >{{pageNumber-1}}</span>

    <span :class="['Pagination-Item', 'Pagination-SelectedPage']">&#8195;{{pageNumber}}&#8195;</span>

    <span
      v-if="pageCount - pageNumber > 1"
      @click="$emit('update:page-number', pageNumber+1)"
      class="Pagination-Item"
    >{{pageNumber+1}}</span>

    <span v-if="pageCount - pageNumber > 2">&#8195;...&#8195;</span>

    <span
      v-if="pageNumber < pageCount"
      @click="$emit('update:page-number', pageCount)"
      class="Pagination-Item"
    >&#8195;{{pageCount}}</span>

    <span
      @click="$emit('update:page-number', pageNumber < pageCount ? pageNumber+1 : pageNumber)"
      class="Pagination-Item"
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
  text-align: center;

  &-Item {
    padding: 3px;
    font-size: 16px;
    cursor: pointer;
  }

  &-SelectedPage {
    cursor: pointer;
    color: red;
  }
}
</style>
