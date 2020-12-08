<template>
  <div
    class="mt-3 text-left row"
    v-bind:style="{
      width: containerWidth,
    }"
  >
    <span class="col ml-3"
      >Showing {{ (currentPage - 1) * pageSize + 1 }} -
      {{
        currentPage * pageSize > totalRecords
          ? totalRecords
          : currentPage * pageSize
      }}
      of {{ totalRecords }} entries</span
    >
    <span>
      <span
        class="page-cell hover-highlight hover-pointer"
        v-if="currentPage - 4 >= 0"
        @click="goToPage(startPage)"
        >{{ startPage }}</span
      >
      <span class="page-cell" v-if="currentPage - 4 >= 0">...</span>
      <span
        class="page-cell hover-highlight hover-pointer"
        v-if="currentPage - 2 > 0"
        @click="goToPage(currentPage - 2)"
        >{{ currentPage - 2 }}</span
      >
      <span
        class="page-cell hover-highlight hover-pointer"
        v-if="currentPage - 1 > 0"
        @click="goToPage(currentPage - 1)"
        >{{ currentPage - 1 }}</span
      >
      <span class="page-cell current-page">{{ currentPage }}</span>
      <span
        class="page-cell hover-highlight hover-pointer"
        v-if="currentPage < endPage"
        @click="goToPage(currentPage + 1)"
        >{{ currentPage + 1 }}</span
      >
      <span
        class="page-cell hover-highlight hover-pointer"
        v-if="currentPage + 1 < endPage"
        @click="goToPage(currentPage + 2)"
        >{{ currentPage + 2 }}</span
      >
      <span class="page-cell" v-if="currentPage + 3 <= endPage">...</span>
      <span
        class="page-cell hover-highlight hover-pointer"
        v-if="currentPage + 3 <= endPage"
        @click="goToPage(endPage)"
        >{{ endPage }}</span
      >
    </span>
  </div>
</template>

<script>
export default {
  name: "PaginationComponent",
  props: {
    containerWidth: String,
    pageSize: Number,
    totalRecords: Number,
    currentPage: Number,
  },

  data() {
    return {
      startPage: 1,
      endPage: Math.ceil(this.totalRecords / this.pageSize),
    };
  },

  methods: {
    goToPage(pageNo) {
      this.$emit("gotopage", pageNo);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.page-cell {
  border: 1px solid #d5dbdb;
  margin: 0 5px;
  padding: 5px 12px;
}

.current-page,
.hover-highlight:hover {
  font-weight: bold;
  background: #3498db;
  color: white;
}
</style>
