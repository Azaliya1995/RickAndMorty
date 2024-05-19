<template>
  <div>
    <FilterComponent :filters='filters' @update:filters='handleFilterUpdate' />
    <div class='character-grid'>
      <CharacterCard v-for='character in filteredCharacters' :key='character.id' :character='character' />
    </div>
    <Pagination
      :currentPage='currentPage'
      :totalPages='totalPages'
      :nextUrl='nextUrl'
      :prevUrl='prevUrl'
      :firstPageUrl='firstPageUrl'
      :lastPageUrl='lastPageUrl'
      :fetchCharacters='fetchCharacters'
    />
  </div>
</template>

<script>
import CharacterCard from './components/CharacterCard.vue';
import Pagination from './components/PaginationComponent.vue';
import FilterComponent from './components/FilterComponent.vue';
import { ref, onMounted, computed } from 'vue';
import { API_URL } from './config.js';

export default {
  name: 'MainComponent',
  components: {
    CharacterCard,
    Pagination,
    FilterComponent
  },
  setup() {
    const characters = ref([]);
    const currentPage = ref(1);
    const totalPages = ref(1);
    const nextUrl = ref(null);
    const prevUrl = ref(null);
    const firstPageUrl = ref(API_URL);
    const lastPageUrl = ref(null);
    const filters = ref({
      name: '',
      status: ''
    });

    const handleFilterUpdate = (newFilters) => {
      filters.value = { ...newFilters };
      fetchCharacters();
    };

    const buildUrl = (baseUrl, filters) => {
      const url = new URL(baseUrl);
      if (filters.name) {
        url.searchParams.append('name', filters.name);
      }
      if (filters.status) {
        url.searchParams.append('status', filters.status);
      }
      return url.toString();
    };

    const fetchCharacters = async (url = API_URL) => {
      try {
        const apiUrl = buildUrl(url, filters.value);
        const response = await fetch(apiUrl);
        const data = await response.json();
        characters.value = data.results;
        currentPage.value = apiUrl.includes('page=') ? parseInt(new URL(apiUrl).searchParams.get('page')) : 1;
        totalPages.value = data.info.pages;
        nextUrl.value = data.info.next;
        prevUrl.value = data.info.prev;
        lastPageUrl.value = buildUrl(`${url}?page=${data.info.pages}`, filters.value);
      } catch (error) {
        console.error('Error fetching characters:', error);
      }
    };

    onMounted(() => {
      fetchCharacters();
    });

    const filteredCharacters = computed(() => {
      return characters.value.filter(character => {
        const nameFilter = filters.value.name.toLowerCase();
        const statusFilter = filters.value.status.toLowerCase();
        return (
          character.name.toLowerCase().includes(nameFilter) &&
          (statusFilter === '' || character.status.toLowerCase() === statusFilter)
        );
      });
    });

    return {
      characters,
      currentPage,
      totalPages,
      nextUrl,
      prevUrl,
      firstPageUrl,
      lastPageUrl,
      filters,
      handleFilterUpdate,
      fetchCharacters,
      filteredCharacters
    };
  }
};
</script>

<style scoped>
.character-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 16px;
  max-width: 1200px;
}

img {
  max-width: 100%;
  height: auto;
}
</style>
