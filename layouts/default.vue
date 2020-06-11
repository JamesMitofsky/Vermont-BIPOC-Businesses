<template>
  <v-app dark>
    <v-app-bar fixed app>
      <nuxt-link class="unstyled-link" to="/">
        <v-toolbar-title v-text="title" />
      </nuxt-link>
    </v-app-bar>

    <!-- catch injected content -->
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>

    <!-- footer here -->
    <v-footer app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  
  head: {
    title: 'Vermont BIPOC Business'
  },

  data() {
    return {
      title: 'Vermont BIPOC Businesses'
    }
  }
}
</script>

<style>
.unstyled-link {
  text-decoration: none;
}

</style>
