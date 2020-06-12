<template>
  <v-layout column justify-center align-center>
    <v-card class="col-12">
      <v-card-title>Support BIPOC</v-card-title>
      <v-card-text>BIPOC is an acronym for Black, Indigenous, and People of Color. During this pandemic — and especially during a time when the validity of black lives are under extra stress — consider giving your patronage to one of these business owners.</v-card-text>
    </v-card>

    <v-autocomplete
      class="align-self-start"
      type="text"
      :search-input.sync="search"
      :items="filteredItems"
      hide-no-data
      append-icon=""
      hide-details
      hide-selected
      clearable
      prepend-icon="mdi-database-search"
      placeholder="Search Businesses"
    ></v-autocomplete>
    <v-divider></v-divider>
    <v-expand-transition>
      <!-- Filtered list -->
      <v-list class="align-self-start">
        <v-list-item v-for="item in filteredItems" :key="item.slug" :to="item.path">
          <v-list-item-content v-if="item.businessName || item.ownerName">
            <v-list-item-title v-if="item.businessName">{{item.businessName}}</v-list-item-title>
            <v-list-item-title v-else-if="item.ownerName">{{item.ownerName}}</v-list-item-title>
            <v-list-item-subtitle>{{item.description}}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-expand-transition>

    <!-- All businesses -->
    <h1 class="align-self-start display-1">All Businesses:</h1>
    <v-list class="align-self-start col-12">
      <v-list-item v-for="doc in docs" :key="doc.slug" :to="doc.path">
        <span v-if="doc.businessName">{{doc.businessName}}</span>
        <span v-else-if="doc.ownerName">{{doc.ownerName}}</span>
        <span v-if="doc.businessName || doc.ownerName" class="mx-2">—</span>
        <span class="subtitle-2">{{doc.description}}</span>
      </v-list-item>
    </v-list>
  </v-layout>
</template>

<script>
export default {
  data() {
    return {
      search: ''
    }
  },

  computed: {
    filteredItems() {
      return this.docs.filter(doc => {
        if (this.search != '') {
          let localSearch = new RegExp(this.search, 'gi')
          return JSON.stringify(doc).match(localSearch)
        }
      })
    }
  },

  async asyncData({ $content, params }) {
    const docs = await $content('business').fetch()
    return { docs }
  },

  created() {},

  mounted() {
    if (window.netlifyIdentity) {
      // console.log(this.docs)

      // check if user is admin
      window.netlifyIdentity.on('init', user => {
        if (!user) {
          window.netlifyIdentity.on('login', () => {
            document.location.href = '/admin/'
          })
        }
      })
    }
  }
}
</script>

<style scoped>
nuxt-link {
  padding: 100px;
}
</style>
