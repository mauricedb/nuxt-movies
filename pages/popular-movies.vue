<template>
  <div>
    Top Rated Movies
    <MovieList :movies-url="moviesUrl" title="Popular Movies" type="popular" />
  </div>
</template>

<script>
import MovieList from '../components/MovieList.vue'
export default {
  components: { MovieList },
  computed: {
    moviesUrl() {
      return `${process.env.apiOrigin}/popular-movies`
    },
  },
}
</script>
