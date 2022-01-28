<template>
  <v-app>
    

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
              href="https://faucet.chainscore.finance/"
              target="_blank"
              dark
              rounded
              color="yellow darken-4"
              large
              >$SCORE Faucet</v-btn
            >
          </v-list-item>

          <v-list-item>
            <v-btn class="ma-4" to="/harmony" dark large rounded color="green"
        >Try on Harmony Testnet</v-btn
      >
          </v-list-item>

          <v-list-item>
            <v-btn class="ma-4" to="/" light large rounded color="white"
        >Try on ChainScore Network</v-btn
      >
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
      flat
      class="py-2"
      color="transparent"
      
    >
      <img src="~/assets/logo.png" width="200"  />

      <v-spacer></v-spacer>

          <a
            text
            href="https://docs.chainscore.finance"
            target="_blank"
            class="yellow--text text-button my-2"
            
            >docs</a
          >

        <v-app-bar-nav-icon class="mr-5" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

    </v-app-bar>

    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>

    <div class="d-flex flex-column justify-center align-center pb-15 mb-10">
      <div class="text--secondary my-5">Supported Protocols</div>
      <div class="d-flex mt-5 flex-wrap justify-center">
        <!-- <v-img
          class="mx-10 my-5"
          src="https://cdn-images-1.medium.com/max/1200/1*uOUrHJwAlNPBiGzl3slkWw.png"
          style="opacity: 0.7;"
          max-width="80"
          contain
        ></v-img>
        <v-img
          class="mx-10 my-5"
          src="https://cryptologos.cc/logos/chainlink-link-logo.png?v=002"
          style="opacity: 0.7"
          max-width="80"
          contain
        ></v-img>
        <v-img
          class="mx-10 my-5"
          src="https://chainstack.com/wp-content/uploads/2020/06/covalent.png"
          style="opacity: 0.7"
          max-width="80"
          contain
        ></v-img> -->
        <v-img
          class="mx-10 "
          src="https://cryptologos.cc/logos/compound-comp-logo.png"
          style="opacity: 0.7"
          max-width="80"
          contain
        ></v-img>
        <v-img
          class="mx-10 "
          src="https://cryptologos.cc/logos/aave-aave-logo.png"
          style="opacity: 0.7"
          max-width="80"
          contain
        ></v-img>
      </div>
    </div>
    <v-footer app color="yellow darken-1 pa-1">
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
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js',
    }
  },
}
</script>
