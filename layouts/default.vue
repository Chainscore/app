<template>
  <v-app >

    <v-app-bar

      :clipped-left="clipped"
      fixed
      app
      flat
      color="white"
      class="my-3"
    >
      <img src="~/assets/logo.png" width="200"/>

      <v-spacer></v-spacer>
      <div class="mr-10"><v-btn text href="https://docs.chainscore.finance" target="_blank">documentation</v-btn></div>

      <v-btn class="mr-4" color="blue" rounded dark>$ONE Faucet</v-btn>
      <v-btn class="mr-4" outlined rounded color="yellow darken-4">Swap $SCORE for $ONE</v-btn>
      
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>

    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
      <v-spacer></v-spacer>
      <div class="mr-5"><a href="https://docs.chainscore.finance" target="_blank">Twitter</a></div>

      <div class="mr-10"><a href="https://docs.chainscore.finance" target="_blank">Discord</a></div>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js'
    }
  }
}
</script>
