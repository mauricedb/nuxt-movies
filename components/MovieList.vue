<template>
  <div>
    <h2>{{ title }}</h2>
    <div>
      <div v-if="error">{{ error }}</div>
      <div v-else-if="loading">Loading...</div>
      <v-row v-else>
        <MovieCard
          v-for="movie in movies"
          :key="movie.id"
          :movie="movie"
          :type="type"
        />
      </v-row>
    </div>
  </div>
</template>

<script>
import MovieCard from './MovieCard.vue'

export default {
  components: {
    MovieCard,
  },
  props: {
    moviesUrl: {
      type: String,
      required: true,
    },
    title: {
      type: String,
      required: true,
    },
    type: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      error: null,
      loading: true,
      movies: [],
    }
  },
  watch: {
    moviesUrl() {
      this.fetchMovies()
    },
  },
  mounted() {
    this.fetchMovies()
  },
  methods: {
    async fetchMovies() {
      try {
        const rsp = await fetch(this.moviesUrl)

        if (rsp.ok) {
          this.movies = await rsp.json()
        } else {
          this.error = rsp.statusText ?? 'Failed to load data'
        }
      } catch (error) {
        this.error = error?.message ?? 'Failed to load data'
      } finally {
        this.loading = false
      }
    },
  },
}
</script>

<style scoped></style>
