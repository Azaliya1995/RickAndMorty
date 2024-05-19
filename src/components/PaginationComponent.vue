<template>
  <div class="pagination">
    <button @click="fetchCharacters(firstPageUrl)" :disabled="!prevUrl">First</button>
    <button @click="fetchCharacters(prevUrl)" :disabled="!prevUrl">Previous</button>
    <span>Page {{ currentPage }} of {{ totalPages }}</span>
    <button @click="fetchCharacters(nextUrl)" :disabled="!nextUrl">Next</button>
    <button @click="fetchCharacters(lastPageUrl)" :disabled="!nextUrl">Last</button>
  </div>
</template>

<script>
export default {
  props: {
    currentPage: Number,
    totalPages: Number,
    nextUrl: String,
    prevUrl: String,
    firstPageUrl: String,
    lastPageUrl: String,
    fetchCharacters: Function
  }
};
</script>

<style scoped>
.pagination {
  margin: 16px 0;
  text-align: center;
}

button {
  margin: 0 8px;
}
</style>
