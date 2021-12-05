<template>
  <div>
    <div class="ma-15" style="padding-top: 8%">
      <div class="text-h1 font-weight-medium text-center letter-spacing-10" style="line-height: 1.2;">
        ChainScore in now live <div class="text-h1 harmony">on Harmony Testnet</div> 
      </div>
    </div>

    <div class="ma-15 py-5">
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
        >
        </v-text-field>

        <v-btn
          dark
          class="mx-4"
          fab
          elevation="0"
          color="yellow darken-1"
          :disabled="status=='loading'"
          @click="requestScore()"
        >
          <v-icon> mdi-flash </v-icon>
        </v-btn>
      </div>

      <div v-else class="text-center my-10 py-5">
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

    <div class="ma-15" style="padding: 1% 8%;" v-if="status=='done'">
      <div class="d-flex align-center mt-15 my-10 justify-space-around">
        <div class="text-h4 font-weight-bold">Overall Score</div>
        <div class="text-h2 mx-15 font-weight-bold">{{score * 100}}</div>
      </div>

      <v-progress-linear height="20" :value="score * 100" color="yellow darken-1"></v-progress-linear>

      <div class="d-flex align-center mt-15 mb-10 justify-space-around text--secondary">
        <div class="text-h5">Supply Score</div>
        <div class="text-h4 mx-15">{{supply_score * 100}}</div>
      </div>
  
    <v-progress-linear height="10" :value="supply_score * 100" color="yellow darken-1"></v-progress-linear>

      <div class="d-flex align-center my-10 justify-space-around text--secondary">
        <div class="text-h5">Valuation Score</div>
        <div class="text-h4 mx-15">{{value_score * 100}}</div>
      </div>

    <v-progress-linear height="10" :value="value_score * 100" color="yellow darken-1"></v-progress-linear>

      <div class="d-flex align-center my-10 justify-space-around text--secondary">
        <div class="text-h5">Debt Score</div>
        <div class="text-h4 mx-15">{{debt_score * 100}}</div>
      </div>

    <v-progress-linear height="10" :value="debt_score * 100" color="yellow darken-1"></v-progress-linear>

      <div class="d-flex align-center my-10 justify-space-around text--secondary">
        <div class="text-h5">Repayment Score</div>
        <div class="text-h4 mx-15">{{repayment_score * 100}}</div>
      </div>

    <v-progress-linear height="10" :value="repayment_score * 100" color="yellow darken-1"></v-progress-linear>
    </div>

    <div class="d-flex justify-center" v-else-if="status=='loading'" >
      <v-progress-circular size="150" width="20" color="yellow darken-1" indeterminate></v-progress-circular>
    </div>

    <div></div>
  </div>
</template>

<script>
// const Web3 = require("web3");
const axios = require('axios');

export default {
  components: {
    // Web3ModalVue,
  },
  data() {
    return {
      address: '',
      score: 0,
      supply_score: 0,
      value_score: 0,
      repayment_score: 0,
      debt_score: 0,

      error: null,
      status: "not_req",
      accounts: [],
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

    async requestScore() {
      this.status = "loading";
      try{
        const resp = await axios.get(`http://169.60.167.178:3001/score/${this.address}`)
        console.log(resp);
        
        this.score = resp.data.score.toFixed(2);
        this.supply_score = resp.data.supply_score.toFixed(2);
        this.value_score = resp.data.value_score.toFixed(2);
        this.repayment_score = resp.data.repayment_score.toFixed(2);
        this.debt_score = resp.data.debt_score.toFixed(2);

        this.status = "done";
    }
      catch(err) {
        this.status = "errored";
        this.error = err;
      }
    }
  },
}
</script>

<style lang="css">
.harmony {
  background: -webkit-linear-gradient(right, #1cd8d2, #93edc7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>