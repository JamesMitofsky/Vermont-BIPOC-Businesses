<template>
  <div>
    <v-card class="mb-5">
      <v-card-title v-if="doc.businessName">{{doc.businessName}}</v-card-title>
      <v-card-title v-if="doc.ownerName">{{doc.ownerName}}</v-card-title>
      <v-card-subtitle>{{doc.type}}</v-card-subtitle>

      <v-card-text class="white--text">{{doc.description}}</v-card-text>

      <v-card class="d-flex text-center text-secondary">
        <v-card-text>
          Located in
          <a
            target="_blank"
            :href="'https://www.google.com/maps/search/?api=1&query=' + doc.location"
          >{{doc.location}}</a>
        </v-card-text>

        <v-card-text>
          <a target="_blank" :href="'//' + doc.website">Business Website</a>
        </v-card-text>
      </v-card>
    </v-card>
    <div class="d-flex justify-center">
      <v-btn class="ma-5" v-if="prev">
        <nuxt-link :to="prev.slug">Previous</nuxt-link>
      </v-btn>
      <v-btn class="ma-5" v-if="next">
        <nuxt-link :to="next.slug">Next</nuxt-link>
      </v-btn>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const doc = await $content(`business/${params.slug}` || 'index').fetch()
    const [prev, next] = await $content(`business`)
      .only(['title', 'slug'])
      .sortBy('title', 'asc')
      .surround(params.slug, { before: 1, after: 1 })
      .fetch()
    return { doc, prev, next }
  }
}
</script>

<style>
</style>