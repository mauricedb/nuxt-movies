<template>
  <v-card elevation="3" class="mx-auto my-3" max-width="500">
    <nuxt-link :to="{ name: 'movie-type-id', params: { id: movie.id, type } }">
      <v-img :src="imageUrl" height="281" width="500" />
      {{ movie.title }}
      <!-- <span :title="movie.vote_average">
        ({{ movie.vote_average | stars }})
      </span> -->
      <v-rating
        :value="movie.vote_average / 2"
        :title="movie.vote_average"
        readonly
        size="x-small"
      />
    </nuxt-link>
  </v-card>
</template>

<script>
export default {
  filters: {
    stars(value) {
      return ''.padEnd(value / 2, '★').padEnd(5, '☆')
    },
  },
  props: {
    movie: Object,
    type: String,
  },
  computed: {
    imageUrl() {
      return 'https://image.tmdb.org/t/p/w500' + this.movie.backdrop_path
    },
  },
}
</script>

<style scoped></style>
