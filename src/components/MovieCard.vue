<template>
  <div
    class="bg-white shadow-lg rounded-lg overflow-hidden transition-transform transform hover:scale-105 hover:shadow-xl"
  >
    <img
      :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path"
      alt="Movie Poster"
      class="w-full h-auto"
    />
    <div class="p-4">
      <h3 class="truncate">{{ movie.original_title }}</h3>
      <p class="text-gray-600">{{ formattedDate }}</p>
    </div>
  </div>
</template>

<script setup>
import { format, parseISO } from 'date-fns'

const props = defineProps({
  movie: Object,
})

const parsedDate = parseISO(props.movie.release_date)
const formattedDate = formatDate(parsedDate)

function formatDate(date) {
  return format(date, 'MMMM d, yyyy')
}
</script>
