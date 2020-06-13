<template>
  <div column justify-center align-center>
    <v-card class="col-12">
      <v-card-title>Support BIPOC</v-card-title>
      <v-card-text>BIPOC is an acronym for Black, Indigenous, and People of Color. During this pandemic — and especially during a time when the validity of black lives are under extra stress — consider giving your patronage to one of these business owners.</v-card-text>
    </v-card>

    <v-autocomplete
      class="align-self-start my-10"
      type="text"
      :search-input.sync="search"
      :items="filteredItems"
      hide-no-data
      append-icon
      hide-details
      hide-selected
      clearable
      prepend-icon="mdi-database-search"
      placeholder="Search Businesses"
    ></v-autocomplete>
    <!-- Filtered list -->
    <v-list v-if="search != null && search != ''" class="align-self-start col-12 mt-n8 mb-10">
      <v-list-item
        class="primary lighten4"
        v-for="item in filteredItems"
        :key="item.slug"
        :to="item.path"
      >
        <v-list-item-content v-if="item.businessName || item.ownerName">
          <v-list-item-title v-if="item.businessName">{{item.businessName}}</v-list-item-title>
          <v-list-item-title v-else-if="item.ownerName">{{item.ownerName}}</v-list-item-title>
          <v-list-item-subtitle class="col-10">{{item.description}}</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
      <v-list-item v-show="filteredItems.length == 0">
        <v-list-item-content>
          <v-list-item-title>Sorry, we didn't find a match.</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>

    <!-- All businesses -->
    <v-row class="d-flex custom-align mr-auto">
      <h1 class="display-1 mr-10">Businesses:</h1>
      <v-select :items="businessTypes" v-model="selectedType" label="Filter"></v-select>
    </v-row>

    <v-list class="col-12">
      <v-list-item v-for="type in typeSorting" :key="type.slug" :to="type.path">
        <v-list-item-content v-if="type.businessName || type.ownerName">
          <v-list-item-title v-if="type.businessName">{{type.businessName}}</v-list-item-title>
          <v-list-item-title v-else-if="type.ownerName">{{type.ownerName}}</v-list-item-title>
          <v-list-item-subtitle>{{type.description}}</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </div>
</template>

<script>
export default {
  data() {
    return {
      search: '',
      selectedType: '',
      businessTypes: [
        'Show All',
        'Musician/Performer/Artist',
        'Barber/Stylist',
        'Graphic Design',
        'Healthcare Provider',
        'Market/Grocer',
        'Educator/Consultant',
        'Restaurant/Coffee Shop/Bar',
        'Retail',
        'Trades',
        'Other'
      ]
    }
  },

  computed: {
    filteredItems() {
      return this.docs.filter(doc => {
        if (this.search != '') {
          let localSearch = new RegExp('\\b' + this.search, 'gi')
          return JSON.stringify(doc).match(localSearch)
        }
      })
    },

    typeSorting() {
      return this.docs.filter(doc => {
        if (doc.type.match(this.selectedType)) {
          return doc
        } else if (this.selectedType == 'Show All') {
          return doc
        }
      })
    }
  },

  async asyncData({ $content, params }) {
    const docs = await $content('business').fetch()
    return { docs }
  },

  mounted() {
    // check if user is admin
    if (window.netlifyIdentity) {
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
.custom-align {
  align-items: center;
}
</style>
