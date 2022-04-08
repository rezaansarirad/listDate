<template>
  <v-app class="bg">
    <v-main>
      <v-container fluid>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  data() {
    return {};
  },
};
</script>

<style scoped>
.bg {
  background: #f8f8f8;
}
</style>
