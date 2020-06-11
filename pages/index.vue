<template>
  <v-layout column justify-center align-center>
    <v-card class="col-12">
      <v-card-title>Support Vermont's BIPOC Community</v-card-title>
      <v-card-text>BIPOC is an acronym for Black, Indigenous, and People of Color. During this pandemic — and especially during a time when the validity of black lives are under extra stress — consider giving your patronage to one of these business owners.</v-card-text>
    </v-card>

    <h1 class="mt-10 align-self-start display-1">Businesses:</h1>
    <v-list class="align-self-start col-12">
      <v-list-item v-for="doc in docs" :key="doc.slug" :to="doc.path">
        <span>{{doc.title}}</span>
        <span class="mx-2">—</span>
        <span class="subtitle-2">{{doc.type}}</span>
      </v-list-item>
    </v-list>
  </v-layout>
</template>

<script>
export default {
  
  mounted() {
    if (window.netlifyIdentity) {
      window.netlifyIdentity.on('init', user => {
        if (!user) {
          window.netlifyIdentity.on('login', () => {
            document.location.href = '/admin/'
          })
        }
      })
    }
  },
  
  async asyncData({ $content, params }) {
    const docs = await $content('business').fetch()
    return { docs }
  }
}
</script>

<style scoped>
nuxt-link {
  padding: 100px;
}
</style>
