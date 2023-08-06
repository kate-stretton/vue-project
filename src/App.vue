<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import { debounce } from 'lodash'
import MovieCard from './components/MovieCard.vue'

const movies = ref([])
const searchQuery = ref('')
const loading = ref(false)
const currentPage = ref(1)
const totalPages = ref(1)

const changePage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page
    getMovies()
  }
}

const getMovies = () => {
  loading.value = true

  const pageQuery = `&page=${currentPage.value}`

  const apiURL =
    searchQuery.value === ''
      ? `https://api.themoviedb.org/3/discover/movie?api_key=136d55d9c6878da748d19b6aa4870c86${pageQuery}`
      : `https://api.themoviedb.org/3/search/movie?api_key=136d55d9c6878da748d19b6aa4870c86&query=${searchQuery.value}${pageQuery}`

  axios
    .get(apiURL)
    .then((response) => {
      movies.value = response.data.results
      currentPage.value = response.data.page
      totalPages.value = response.data.total_pages
    })
    .finally(() => {
      loading.value = false
    })
}

const onSearchInput = () => {
  debouncedGetMovies()
}

const debouncedGetMovies = debounce(getMovies, 300)

onMounted(() => {
  getMovies()
})
</script>

<template>
  <div class="p-16 bg-yellow-100">
    <h1 class="text-center">Movies</h1>
    <div class="flex justify-center mt-10 mb-10">
      <div class="relative w-full max-w-md">
        <div
          class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none"
        >
          <svg
            class="w-4 h-4"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 20 20"
          >
            <path
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"
            />
          </svg>
        </div>
        <input
          v-model="searchQuery"
          @input="onSearchInput"
          type="text"
          id="search"
          name="search"
          class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-700 focus:border-blue-700 block pl-10 p-3 w-full"
          placeholder="Search by title..."
        />
      </div>
    </div>

    <div
      class="grid grid-cols-1 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-10"
    >
      <template v-if="loading">
        <p>Loading...</p>
      </template>
      <template v-else>
        <MovieCard
          v-for="movie in movies"
          :key="movie.id"
          :movie="movie"
          class="bg-white shadow-lg rounded-lg overflow-hidden"
        />
      </template>
    </div>
    <div class="flex justify-center mt-10">
      <button
        v-if="currentPage > 1"
        :disabled="currentPage === 1"
        @click="changePage(currentPage - 1)"
        class="px-4 py-2 mx-1 bg-blue-900 text-white rounded-md"
      >
        Previous
      </button>
      <button
        :disabled="currentPage === totalPages"
        @click="changePage(currentPage + 1)"
        class="px-4 py-2 mx-1 bg-blue-900 text-white rounded-md"
      >
        Next
      </button>
    </div>
  </div>
</template>
