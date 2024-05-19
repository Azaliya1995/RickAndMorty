<template>
  <div class='character-card'>
    <h2>{{ character.name }}</h2>
    <img :src='character.image' :alt='character.name' />
    <p>Status: {{ character.status }}</p>
    <p>Species: {{ character.species }}</p>
    <p>Gender: {{ character.gender }}</p>
    <p>Origin: {{ character.origin.name }}</p>
  </div>
</template>

<script>
export default {
  props: {
    character: {
      type: Object,
      required: true
    }
  }
};
</script>

<style scoped>
.character-card {
  border: 1px solid #ccc;
  padding: 16px;
  text-align: center;
}

img {
  max-width: 100%;
  height: auto;
}
</style>
