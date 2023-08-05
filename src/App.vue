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
  <div>
    <h1>Movies</h1>
    <div class="mt-10 mb-10">
      <label for="search" class="block text-sm font-medium text-gray-900"
        >Search by title</label
      >
      <input
        v-model="searchQuery"
        @input="getMovies"
        type="text"
        name="search"
        id="search"
        class="border"
      />
    </div>
    <div
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4"
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
