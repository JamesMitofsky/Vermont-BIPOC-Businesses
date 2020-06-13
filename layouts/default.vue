<template>
  <v-app dark>
    <v-app-bar fixed app>
      <nuxt-link class="link" to="/">
        <v-toolbar-title v-text="title" />
      </nuxt-link>
      <v-spacer></v-spacer>
      <nuxt-link class="link" to="/">
        <v-btn class="white blue--text mr-5">Home</v-btn>
      </nuxt-link>
      <nuxt-link class="link" to="/FAQ">
        <v-btn class="white blue--text">FAQ</v-btn>
      </nuxt-link>
    </v-app-bar>

    <!-- catch injected content -->
    <v-content>
      <v-container class="col-lg-8">
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
    title: 'Vermont BIPOC Businesses',
    script: [
      { src: 'https://identity.netlify.com/v1/netlify-identity-widget.js' }
    ]
  },

  data() {
    return {
      title: 'VT BIPOC Owned'
    }
  }
}
</script>

<style>
.link {
  text-decoration: none;
}

.page-enter-active,
.page-leave-active {
  transition: all 0.2s ease;
}
.page-enter,
.page-leave-to {
  opacity: 0;
  transform: translateX(100vw);
}
</style>
