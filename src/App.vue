<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import MovieCard from './components/MovieCard.vue'

const allMovies = ref([])

const getMovies = () => {
  axios
    .get(
      `https://api.themoviedb.org/3/discover/movie?api_key=136d55d9c6878da748d19b6aa4870c86&`
    )
    .then((response) => {
      allMovies.value = response.data.results
    })
}

onMounted(() => {
  getMovies()
})
</script>

<template>
  <div>Movies</div>
  <div
    class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4"
  >
    <MovieCard
      v-for="movie in allMovies"
      :key="movie.id"
      :movie="movie"
      class="bg-white shadow-lg rounded-lg overflow-hidden"
    />
  </div>
</template>
