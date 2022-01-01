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
      <img src="~/assets/logo.png" width="200" style="margin: 10px" />

      <v-spacer></v-spacer>


          <v-btn
            text
            href="https://docs.chainscore.finance"
            target="_blank"
            rounded
            large
            >docs</v-btn
          >

        <v-app-bar-nav-icon class="mr-5" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

    </v-app-bar>

    <v-navigation-drawer v-model="drawer" absolute bottom temporary right width="330" color="yellow">
      <v-list nav dense>
        <v-list-item-group
          v-model="group"
          active-class="deep-yellow--text text--accent-4"
          color="yellow"
        >
          <v-list-item color="blue" class="mt-10">
            <v-btn
              class="ma-4"
              dark
              rounded
              href="https://faucet.pops.one/"
              target="_blank"
              color="blue"
              large
              >$ONE Faucet</v-btn
            >
          </v-list-item>

          <v-list-item>
            <v-btn
              class="ma-4"
              href="https://viperswap.one/#/swap?exactField=input&exactAmount=10&outputCurrency=0x64B60e8C3b8527011B48aD9a19265680FE901CEE"
              target="_blank"
              dark
              rounded
              color="yellow darken-4"
              large
              >Swap $SCORE for $ONE</v-btn
            >
          </v-list-item>

          <v-list-item>
            <v-btn class="ma-4" to="/harmony" dark large rounded color="green"
        >Try on Harmony Testnet</v-btn
      >
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>

    <div class="d-flex flex-column justify-center align-center pb-15 mb-10">
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
          src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/15/Chainlink_Logo_Blue.svg/2560px-Chainlink_Logo_Blue.svg.png"
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
    <v-footer :absolute="!fixed" app color="yellow darken-1 pa-1">
      <span>&copy; {{ new Date().getFullYear() }}</span>
      <v-spacer></v-spacer>
      <div class="mr-5">
        <a href="https://twitter.com/chainscorehq" target="_blank">
          <v-img
            src="https://www.freepnglogos.com/uploads/twitter-logo-png/twitter-logo-vector-png-clipart-1.png"
            width="20"
          ></v-img>
        </a>
      </div>

      <div class="mr-5">
        <a href="https://discord.gg/TaHXKnrgkb" target="_blank">
          <v-img
            src="https://cdn-icons-png.flaticon.com/512/2111/2111370.png"
            width="20"
          ></v-img>
        </a>
      </div>

      <div class="mr-5">
        <a
          class="text-decoration-none"
          href="mailto:hello@chainscore.finance"
          target="_blank"
        >
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
