<template>
  <div class='filters'>
    <input v-model='localFilters.name' placeholder='Filter by name' />
    <select v-model='localFilters.status'>
      <option value=''>All Statuses</option>
      <option value='alive'>Alive</option>
      <option value='dead'>Dead</option>
      <option value='unknown'>Unknown</option>
    </select>
  </div>
</template>

<script>
import { ref, watch } from 'vue';

export default {
  name: 'FilterComponent',
  props: {
    filters: Object,
  },
  setup(props, { emit }) {
    const localFilters = ref({ ...props.filters });

    watch(localFilters, (newValue) => {
      emit('update:filters', newValue);
    }, { deep: true });

    return {
      localFilters
    };
  }
};
</script>

<style scoped>
.filters {
  margin: 16px 0;
}
</style>
