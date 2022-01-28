<template>
  <div>
    <div class="mx-md-15 mx-5 mt-15 pt-15">
      <div
        class="text-h2 text-md-h1 font-weight-bold text-center letter-spacing-10"
      >
        ChainScore in now live
        <div class="text-md-h3 text-h4 mt-5 font-weight-medium harmony">
          on Harmony Testnet
        </div>
      </div>
      <div class="mt-10">
        <div class="text--disabled text-center">
          ✔️ Real-time data from Ethereum Mainnet
        </div>
        <div class="text--disabled text-center">
          ✔️ Supporting ERC20 Assets and NFTs for valuation
        </div>
        <div class="text--disabled text-center">
          ✔️ Credit History (Lending/Borrowing) tracked from Aave V2 and
          Compound
        </div>
      </div>
    </div>

    <div class="ma-md-15 mt-10 px-md-15">
      <div
        v-if="accounts.length > 0"
        class="d-flex justify-center align-bottom text-center mx-5 mx-md-15"
      >
        <v-text-field v-model="address" label="Address (0x)" outlined rounded>
        </v-text-field>

        <v-btn
          dark
          class="mx-4"
          fab
          elevation="0"
          color="yellow darken-1"
          :disabled="status == 'loading'"
          @click="requestScoreFromContract()"
        >
          <v-icon> mdi-flash </v-icon>
        </v-btn>
        <!-- <div>
        <div class="text-h4 orange--text" style="line-height: 1.4">We're sorry, Demo is closed for now!</div>
        <div class="my-5">Join our <a href="https://discord.gg/TaHXKnrgkb">Discord</a> for updates</div>
        </div> -->
      </div>

      <div v-else class="text-center mt-10 pt-5">
        <v-btn
          class="px-10"
          rounded
          light
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
      <div v-if="status == 'errored'" class="text-center text-body-2 red--text">
        {{ error }}
      </div>
    </div>

    <div v-if="status == 'done'" class="pb-10" style="padding: 0% 14%">
      <!-- <div v-if="score != null" class="mx-md-15 mx-5 px-10 mb-5 mt-15"> -->
      <!-- <div v-if="true" class="mx-md-15 mx-5 px-10 mb-5 mt-15"> -->

      <!-- <div class="text-md-h3 text-h4 harmony font-weight-bold pt-10">
        Credit Score
      </div> -->
      <div class="d-flex text-center justify-center ma-5">
        <div>
          <div class="text-md-h4 text-h5 mx-15">Overall Score</div>

          <v-progress-circular
            :rotate="90"
            :size="400"
            :width="80"
            :value="score"
            color="yellow darken-2"
            class="ma-5"
          >
            <div class="text-h3">{{ Math.round(score) }}</div>
          </v-progress-circular>
        </div>
      </div>
      <div class="d-flex flex-wrap justify-space-around text-center">
        <div class="ma-5">
          <div class="text-md-h5 text-h6">Supply Score</div>
          <div class="text-body-2">
            Value deposited/supplied to DeFi protocols
          </div>
          <v-progress-circular
            :rotate="90"
            :size="250"
            :width="50"
            :value="supply_score"
            color="yellow darken-2"
            class="ma-5"
          >
            <div class="text-h4">{{ Math.round(supply_score) }}</div>
          </v-progress-circular>
        </div>
        <div class="ma-5">
          <div class="text-md-h5 text-h6">Value Score</div>
          <div class="text-body-2">
            Based on Account Valuation from assets holding (ERC20)
          </div>
          <v-progress-circular
            :rotate="90"
            :size="250"
            :width="50"
            :value="value_score"
            color="yellow darken-2"
            class="ma-5"
          >
            <div class="text-h4">{{ Math.round(value_score) }}</div>
          </v-progress-circular>
        </div>
        <div class="ma-5">
          <div class="text-md-h5 text-h6">Debt Score</div>
          <div class="text-body-2">
            Depends of debt history or existing debts
          </div>
          <v-progress-circular
            :rotate="90"
            :size="250"
            :width="50"
            :value="debt_score"
            color="yellow darken-2"
            class="ma-5"
          >
            <div class="text-h4">{{ Math.round(debt_score) }}</div>
          </v-progress-circular>
        </div>
        <div class="ma-5">
          <div class="text-md-h5 text-h6">Repayment Score</div>
          <div class="text-body-2">Based on debt repayments at Lending/Borrowing Platforms</div>
          <v-progress-circular
            :rotate="90"
            :size="250"
            :width="50"
            :value="repayment_score"
            color="yellow darken-2"
            class="ma-5"
          >
            <div class="text-h4">{{ Math.round(repayment_score) }}</div>
          </v-progress-circular>
        </div>
      </div>
    </div>

    <div v-else-if="status == 'loading'">
      <!-- <div v-else-if="true"> -->
      <div>
        <div class="d-flex justify-center">
          <v-progress-circular
            size="150"
            width="10"
            color="yellow darken-1"
            indeterminate
          >
          </v-progress-circular>
        </div>
        <div class="text-body-2 text-center my-5" style="color: orange">
          {{ tx_status }}
        </div>
        {{ tx_id }}
      </div>
    </div>
  </div>
</template>

<script>
const Web3 = require('web3')

const options = {
  // Enable auto reconnection
  reconnect: {
    auto: true,
    delay: 5000, // ms
    maxAttempts: 5,
    onTimeout: false,
  },
}
const ws = new Web3.providers.WebsocketProvider('wss://ws.s0.pops.one', options)
const ChainScoreClientJSON = require('../assets/ChainScoreClient.json')
let web3 = new Web3(window.ethereum)

export default {
  components: {
    // Web3ModalVue,
  },
  data() {
    return {
      address: '',
      score: '0',
      supply_score: '0',
      value_score: '0',
      repayment_score: '0',
      debt_score: '0',

      error: null,
      status: 'not_req',
      tx_status: 'not_req',
      tx_id: '',
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

    hasEthereum() {
      const { ethereum } = window
      if (ethereum && ethereum.isMetaMask) {
        return true
      } else {
        return false
      }
    },

    requestScoreFromContract() {
      this.status = 'loading'
      // this.tx_status = 'Confirming Transaction';
      this.tx_status = 'Requesting Score'

      if (!web3.utils.isAddress(this.address)) {
        this.status = 'errored'
        this.error = 'Address not valid'
      } else {
        web3 = new Web3(window.ethereum)
        this.address = this.address.toLowerCase()

        // try {
        const chainScoreClientContract = new web3.eth.Contract(
          ChainScoreClientJSON.abi,
          '0xffC4FeD20b741C7C660aDe359D636AE8A4f307D6'
        )

        chainScoreClientContract.methods
          .requestScore(this.address)
          .send({
            from: this.accounts[0],
          })
          .then((resp) => {
            console.log(resp)
            this.tx_status = 'Request sent! Waiting for score from oracle...'
          })
        // .on('transactionHash', function (hash) {
        //   this.tx_status = 'Score Request sent!'
        //   this.tx_id = hash
        //   console.log(this.tx_status)
        // })
        // .on('receipt', function (receipt) {
        //   // receipt example
        //   this.tx_status = 'Waiting for score from oracle...'
        //   console.log(receipt)
        //   console.log(this.tx_status)
        // })
        // .on('error', (err) => {
        //   this.status = 'errored'
        //   this.error = err.message
        // })

        this.listenToResp(this.address)
        // } catch (err) {
        //   this.status = 'errored'
        //   this.error = err
        //   console.log(err)
        // }
      }
    },

    listenToResp(user) {
      web3 = new Web3(ws, options)

      const myContract = new web3.eth.Contract(
        ChainScoreClientJSON.abi,
        '0xffC4FeD20b741C7C660aDe359D636AE8A4f307D6'
      )

      myContract.events
        .ScoreRequestFulfilled({
          // filter: {user: toString()},
          // fromBlock: 0
        })
        .on('data', (event) => {
          if (user.toLowerCase() === event.returnValues.user.toLowerCase()) {
            this.debt_score = web3.utils.fromWei(
              event.returnValues.debt_score + '00'
            )
            this.score = web3.utils.fromWei(event.returnValues.score + '00')
            this.repayment_score = web3.utils.fromWei(
              event.returnValues.repayment_score + '00'
            )
            this.supply_score = web3.utils.fromWei(
              event.returnValues.supply_score + '00'
            )
            this.value_score = web3.utils.fromWei(
              event.returnValues.value_score + '00'
            )

            this.status = 'done'
            this.tx_status = 'not_req'

            console.log(event)
          }
        })
        .on('error', (error) => {
          console.log(error)
        })
    },
  },
}
</script>

<style lang="css" >
.harmony {
  background: -webkit-linear-gradient(right, #1cd8d2, #93edc7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>
