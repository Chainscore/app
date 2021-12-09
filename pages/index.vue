<template>
  <div>
    <div class="mx-15 mt-10" style="padding-top: 8%">
      <div
        class="text-h3 text-md-h1 font-weight-bold text-center letter-spacing-10"
        style="line-height: 1.2"
      >
        ChainScore in now live
        <div class="font-weight-medium harmony">on Harmony Testnet</div>
      </div>
    </div>

    <div class="ma-md-15 py-5">
      <div
        v-if="accounts.length > 0"
        class="d-flex justify-center align-bottom"
        style="margin: 5% 20%"
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
      </div>

      <div v-else class="text-center mt-10 pt-5">
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
      <div v-if="status == 'errored'" class="text-center text-body-2 red--text">{{ error }}</div>
    </div>

    <div v-if="status == 'done'" class="ma-15" style="padding: 1% 8%">
      <div class="d-flex align-center mt-15 my-10 justify-space-around">
        <div class="text-h4 font-weight-bold">Overall Score</div>
        <div class="text-h2 mx-15 font-weight-bold">
          {{ score.slice(0, 8) }}
        </div>
      </div>

      <v-progress-linear
        height="20"
        :value="parseInt(score)"
        color="yellow darken-1"
      ></v-progress-linear>

      <div
        class="
          d-flex
          align-center
          mt-15
          mb-10
          justify-space-around
          text--secondary
        "
      >
        <div class="text-h5">Supply Score</div>
        <div class="text-h4 mx-15">{{ supply_score.slice(0, 8) }}</div>
      </div>

      <v-progress-linear
        height="10"
        :value="parseInt(supply_score)"
        color="yellow darken-1"
      ></v-progress-linear>

      <div
        class="d-flex align-center my-10 justify-space-around text--secondary"
      >
        <div class="text-h5">Valuation Score</div>
        <div class="text-h4 mx-15">{{ value_score.slice(0, 8) }}</div>
      </div>

      <v-progress-linear
        height="10"
        :value="parseInt(value_score)"
        color="yellow darken-1"
      ></v-progress-linear>

      <div
        class="d-flex align-center my-10 justify-space-around text--secondary"
      >
        <div class="text-h5">Debt Score</div>
        <div class="text-h4 mx-15">{{ debt_score.slice(0, 8) }}</div>
      </div>

      <v-progress-linear
        height="10"
        :value="parseInt(debt_score)"
        color="yellow darken-1"
      ></v-progress-linear>

      <div
        class="d-flex align-center my-10 justify-space-around text--secondary"
      >
        <div class="text-h5">Repayment Score</div>
        <div class="text-h4 mx-15">{{ repayment_score.slice(0, 8) }}</div>
      </div>

      <v-progress-linear
        height="10"
        :value="parseInt(repayment_score)"
        color="yellow darken-1"
      ></v-progress-linear>
    </div>

    <div v-else-if="status == 'loading'" class="d-flex justify-center">
      <div>
      <v-progress-circular
        size="150"
        width="10"
        color="yellow darken-1"
        indeterminate
      > </v-progress-circular>
    <div class="text-body-2 text-center my-5" style="color: orange">{{tx_status}}</div> 
      {{tx_id}}
      </div>
    </div>

    <!-- <div class="text-center" v-else>
      <div class="text-h4">Instructions</div>
      <div class="text-body-1 mt-10">1. Insert your address</div>
      <div class="text-body-1 mt-5 ">2. </div>
      <div class="text-body-1 mt-5 ">3. Lo</div>
      <div class="text-body-1 mt-5 ">4. Lo</div>
      <div class="text-body-1 mt-5 ">5. Lo</div>

    </div> -->

    <div></div>
  </div>
</template>

<script>
const Web3 = require('web3')
const axios = require('axios')

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
      this.tx_status = 'Requesting Score';

      if (!web3.utils.isAddress(this.address)) {
        this.status = 'errored'
        this.error = 'Address not valid'
      } else {
        web3 = new Web3(window.ethereum)
        this.address = this.address.toLowerCase()

        try {
          const chainScoreClientContract = new web3.eth.Contract(
            ChainScoreClientJSON.abi,
            process.env.CHAINSCORE_CLIENT
          )

          chainScoreClientContract.methods
            .requestScore(this.address, process.env.SCORE_MULTIPLE_JOB_SPEC)
            .send({
              from: this.accounts[0],
            })
            .on('transactionHash', function (hash) {
              this.tx_status = 'Score Request sent!';
              this.tx_id = hash;
              console.log(this.tx_status);
            })
            .on('receipt', function (receipt) {
              // receipt example
              this.tx_status = 'Waiting for score from oracle...';
              console.log(receipt)
              console.log(this.tx_status);

            })
            .on('error', (err) =>{
              this.status = 'errored'
              this.error = err.message
            })

          this.listenToResp(this.address)
        } catch (err) {
          this.status = 'errored'
          this.error = err
          console.log(err)
        }
      }
    },

    listenToResp(user) {
      web3 = new Web3(ws, options)

      const myContract = new web3.eth.Contract(
        ChainScoreClientJSON.abi,
        process.env.CHAINSCORE_CLIENT
      )

      myContract.events
        .ScoreRequestFulfilled({
          // filter: {user: toString()},
          // fromBlock: 0
        })
        .on('data', (event) => {
          if (
            user.toLowerCase() === (event.returnValues.user).toLowerCase()
          ) {
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
            this.tx_status = 'not_req';

            console.log(event)
          }
        })
        .on('error', (error) => {
          console.log(error)
        })
    },
    async requestScore() {
      this.status = 'loading'
      if (!web3.utils.isAddress(this.address)) {
        this.status = 'errored'
        this.error = 'Address not valid'
      } else {
        try {
          const resp = await axios.get(
            `http://169.60.167.178:3001/score/${this.address}`
          )
          console.log(resp)

          this.score = resp.data.score.toFixed(2)
          this.supply_score = resp.data.supply_score.toFixed(2)
          this.value_score = resp.data.value_score.toFixed(2)
          this.repayment_score = resp.data.repayment_score.toFixed(2)
          this.debt_score = resp.data.debt_score.toFixed(2)

          this.status = 'done'
        } catch (err) {
          this.status = 'errored'
          this.error = err
        }
      }
    },
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