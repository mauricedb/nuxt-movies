<template>
  <div>
    Top Rated Movies
    <MovieList
      :movies-url="moviesUrl"
      title="Top Rated Movies"
      type="top-rated"
    />
  </div>
</template>

<script>
import MovieList from '../components/MovieList.vue'
export default {
  components: { MovieList },
  computed: {
    moviesUrl() {
      return `${process.env.apiOrigin}/top-rated-movies`
    },
  },
}
</script>
