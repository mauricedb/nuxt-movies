<template>
  <div>
    <h2>{{ title }}</h2>
    <div>
      <div v-if="$fetchState.error">{{ $fetchState.error.message }}</div>
      <div v-else-if="$fetchState.pending">Loading...</div>
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
      movies: [],
    }
  },
  async fetch() {
    const rsp = await fetch(this.moviesUrl)

    if (rsp.ok) {
      this.movies = await rsp.json()
    } else {
      throw new Error(rsp.statusText ?? 'Failed to load data')
    }
  },
}
</script>

<style scoped></style>
