<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
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
  <div>
    <ul>
      <li v-for="movie in allMovies" :key="movie.id">
        {{ movie.original_title }}
      </li>
    </ul>
  </div>
</template>
