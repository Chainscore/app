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
      <div
        v-if="!$vuetify.breakpoint.mobile"
        class="d-flex flex-wrap justify-space-between"
      >
        <div class="mr-10">
          <v-btn
            text
            href="https://docs.chainscore.finance"
            target="_blank"
            rounded
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

    <div
      class="d-flex flex-column justify-center align-center my-15 pt-15 py-5"
    >
      <div class="text--secondary">Data provided by</div>
      <div class="d-flex flex-wrap justify-center">
        <v-img
          class="ma-2"
          src="https://miro.medium.com/max/955/1*UUtSs0Hfsy3M0GCw9k4UCA.jpeg"
          style="opacity: 0.4"
          max-width="200"
          contain
        ></v-img>
        <v-img
          class="ma-2"
          src="https://moonbeam.network/wp-content/uploads/2021/04/covalent-500.png"
          style="opacity: 0.4"
          max-width="200"
          contain
        ></v-img>
        <v-img
          class="ma-2"
          src="https://raw.githubusercontent.com/ajb413/compound-interest-alerts/master/compound-finance-logo.png"
          style="opacity: 0.4"
          max-width="200"
          contain
        ></v-img>
        <v-img
          class="ma-2"
          src="https://miro.medium.com/max/600/0*DdTT4AumgM-dIVzz"
          style="opacity: 0.4"
          max-width="200"
          contain
        ></v-img>
      </div>
    </div>
    <v-footer :absolute="!fixed" app color="yellow darken-1 pa-3">
      <span>&copy; {{ new Date().getFullYear() }}</span>
      <v-spacer></v-spacer>
      <div class="mr-5">
        <a href="https://twitter.com/chainscorehq" target="_blank">
          <v-img
            src="https://www.freepnglogos.com/uploads/twitter-logo-png/twitter-logo-vector-png-clipart-1.png"
            width="30"
          ></v-img>
        </a>
      </div>

      <div class="mr-5">
        <a href="https://discord.gg/TaHXKnrgkb" target="_blank">
          <v-img
            src="https://cdn-icons-png.flaticon.com/512/2111/2111370.png"
            width="30"
          ></v-img>
        </a>
      </div>

      <div class="mr-5">
        <a class="text-decoration-none" href="mailto://hello@chainscore.finance" target="_blank">
          hello@chainscore.finance
        </a>
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
