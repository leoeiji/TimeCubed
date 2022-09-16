<template>
  <v-app dark>
    <v-main>
        <Nuxt />
    </v-main>

    <v-footer app>
      <a 
        class="grey--text text--darken-1 text-decoration-none"
        href="https://github.com/leoeiji"
        target="_blank"
      >@leoeiji</a>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
    }
  }
}
</script>
