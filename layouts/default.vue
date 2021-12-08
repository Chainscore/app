<template>
  <v-app>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
      flat
      color="white"
      class="my-3"
    >
      <img src="~/assets/logo.png" width="200" />

      <v-spacer></v-spacer>
      <div v-if="!$vuetify.breakpoint.mobile" class="d-flex flex-wrap justify-space-between">
        <div class="mr-10">
          <v-btn text href="https://docs.chainscore.finance" target="_blank" rounded
            >documentation</v-btn
          >
        </div>

        <v-btn
          class="mr-4"
          href="https://faucet.pops.one/"
          target="_blank"
          color="blue"
          rounded
          dark
          >$ONE Faucet</v-btn
        >
        <v-btn
          class="mr-4"
          href="https://viperswap.one/#/swap?exactField=input&exactAmount=10&outputCurrency=0x64B60e8C3b8527011B48aD9a19265680FE901CEE"
          target="_blank"
          outlined
          rounded
          color="yellow darken-4"
          >Swap $SCORE for $ONE</v-btn
        >
      </div>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>

    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
      <v-spacer></v-spacer>
      <div class="mr-5">
        <a href="https://twitter.com/chainscorehq" target="_blank">Twitter</a>
      </div>

      <div class="mr-10">
        <a href="https://discord.gg/TaHXKnrgkb" target="_blank">Discord</a>
      </div>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire',
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js',
    }
  },
}
</script>
