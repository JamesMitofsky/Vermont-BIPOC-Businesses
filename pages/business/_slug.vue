<template>
  <div>
    <v-card class="mb-5">
      <v-card-title>{{doc.title}}</v-card-title>
      <v-card-subtitle>{{doc.type}}</v-card-subtitle>

      <v-card-text class="white--text">{{doc.description}}</v-card-text>

      <v-card class="d-flex text-center text-secondary">
        <v-card-text>Weekday Hours: {{doc.weekdayHours}}</v-card-text>
        <v-card-text>Address: {{doc.address}}</v-card-text>
      </v-card>
    </v-card>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const doc = await $content(`business/${params.slug}` || 'index').fetch()
    return { doc }
  }
}
</script>

<style>
</style>