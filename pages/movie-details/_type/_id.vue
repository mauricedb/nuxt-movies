<template>
  <div>
    <div v-if="$fetchState.error">{{ $fetchState.error.message }}</div>
    <div v-else-if="$fetchState.pending">Loading...</div>
    <form v-else @submit.prevent="submitForm" @reset="resetForm" novalidate>
      <fieldset class="fieldset" :disabled="saving">
        <v-text-field v-model="movie.title" label="Title" />
        <v-textarea v-model="movie.overview" label="Overview"></v-textarea>
        <v-text-field
          v-model="movie.vote_average"
          label="Vote average"
          type="number"
        />
        <v-text-field
          v-model="movie.release_date"
          label="Release date"
          type="date"
        />
        <v-btn type="submit">Save</v-btn>
        <v-btn type="reset">Reset</v-btn>
      </fieldset>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      movie: null,
      saving: false,
    }
  },
  async fetch() {
    const rsp = await fetch(this.movieUrl)

    if (rsp.ok) {
      this.movie = await rsp.json()
    } else {
      throw new Error(rsp.statusText ?? 'Failed to load data')
    }
  },
  computed: {
    movieUrl() {
      return `${process.env.apiOrigin}/${this.uriTypeFragment}/${this.$nuxt.$route.params.id}`
    },
    uriTypeFragment() {
      return this.$nuxt.$route.params.type === 'popular'
        ? 'popular-movies'
        : 'top-rated-movies'
    },
  },
  methods: {
    async saveMovie() {
      try {
        this.saving = true
        const rsp = await fetch(this.movieUrl, {
          method: 'put',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(this.movie),
        })

        if (rsp.ok) {
          this.movie = await rsp.json()
        } else {
          this.error = rsp.statusText ?? 'Failed to save data'
        }
      } catch (error) {
        this.error = error?.message ?? 'Failed to save data'
      } finally {
        this.saving = false
      }
    },
    async submitForm() {
      await this.saveMovie()
      if (!this.error) {
        this.$router.push(`/${this.uriTypeFragment}`)
      }
    },
    resetForm() {
      this.$fetch()
    },
  },
}
</script>

<style></style>
