<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import MovieCard from './components/MovieCard.vue'

const movies = ref([])
const searchQuery = ref('')

const getMovies = () => {
  if (searchQuery.value === '') {
    axios
      .get(
        `https://api.themoviedb.org/3/discover/movie?api_key=136d55d9c6878da748d19b6aa4870c86&`
      )
      .then((response) => {
        movies.value = response.data.results
      })
  } else {
    axios
      .get(
        `https://api.themoviedb.org/3/search/movie?api_key=136d55d9c6878da748d19b6aa4870c86&query=${searchQuery.value}`
      )
      .then((response) => {
        movies.value = response.data.results
      })
  }
}

onMounted(() => {
  getMovies()
})
</script>

<template>
  <div class="m-16">
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
          @input="getMovies"
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
      <MovieCard
        v-for="movie in movies"
        :key="movie.id"
        :movie="movie"
        class="bg-white shadow-lg rounded-lg overflow-hidden"
      />
    </div>
  </div>
</template>
