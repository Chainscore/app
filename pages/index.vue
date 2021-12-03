<template>
  <div>
    <div class="ma-15" style="padding-top: 8%">
      <div class="text-h2 text-center letter-spacing-10">
        ChainScore in now live on Harmony Testnet
      </div>
    </div>

    <div class="ma-15">
      <div
        v-if="accounts.length > 0"
        class="d-flex justify-center align-bottom"
        style="margin: 5% 20%"
      >
        <v-text-field
          v-model="address"
          label="Address (0x)"
          outlined
          rounded
          color="yellow darken-1"
        >
        </v-text-field>

        <v-btn
          dark
          class="mx-2"
          fab
          elevation="0"
          color="yellow darken-1"
          @click="requestScore()"
        >
          <v-icon> mdi-flash </v-icon>
        </v-btn>
      </div>

      <div v-else class="text-center">
        <v-btn
          class="px-10"
          rounded
          x-large
          color="yellow darken-1"
          @click="connect()"
          >Connect</v-btn
        >

        <div v-if="error == 'Metamask not installed'">
          <div class="my-5 text-body-1">
            Metamask not installed. Please install it at https://metamask.io/
          </div>
        </div>
      </div>
    </div>

    <div class="ma-15" style="padding: 10%">
      <div class="d-flex align-center justify-space-around">
        <div class="text-h4">Overall Score</div>
        <div class="text-h2 mx-15">{{score}}</div>
      </div>

      <div class="d-flex align-center my-15 justify-space-around">
        <div class="text-h5">Valuation Score</div>
        <div class="text-h4 mx-15">{{score}}</div>
      </div>

      <div class="d-flex align-center my-15 justify-space-around">
        <div class="text-h5">Debt Score</div>
        <div class="text-h4 mx-15">{{score}}</div>
      </div>

      <div class="d-flex align-center my-15 justify-space-around">
        <div class="text-h5">Repayment Score</div>
        <div class="text-h4 mx-15">{{score}}</div>
      </div>
      
    </div>

    <div></div>
  </div>
</template>

<script>
// const Web3 = require("web3");

export default {
  components: {
    // Web3ModalVue,
  },
  data() {
    return {
      address: '',
      web3: null,
      error: null,
      accounts: [],
      score: 0
    }
  },

  methods: {
    async connect() {
      if (this.hasEthereum()) {
        this.accounts = await window.ethereum.request({
          method: 'eth_requestAccounts',
        })

        try {
          await window.ethereum.request({
            method: 'wallet_switchEthereumChain',
            params: [{ chainId: '0x' + (1666700000).toString(16) }],
          })
        } catch (switchError) {
          // This error code indicates that the chain has not been added to MetaMask.
          if (switchError.code === 4902) {
            try {
              await window.ethereum.request({
                method: 'wallet_addEthereumChain',
                params: [
                  {
                    chainName: 'Harmony Testnet',
                    chainId: '0x' + (1666700000).toString(16),
                    rpcUrls: ['https://api.s0.b.hmny.io'],
                    nativeCurrency: {
                      name: 'ONE',
                      symbol: 'ONE',
                      decimals: 18,
                    },
                    blockExplorerUrls: ['https://explorer.pops.one/'],
                  },
                ],
              })
            } catch (addError) {
              // handle "add" error
            }
          }
        }
      } else {
        this.error = 'Metamask not installed'
      }
    },
    addHarmonyTestnet() {
      'wallet_addEthereumChain'
    },

    hasEthereum() {
      const { ethereum } = window
      if (ethereum && ethereum.isMetaMask) {
        return true
      } else {
        return false
      }
    },
  },
}
</script>