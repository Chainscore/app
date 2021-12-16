<template>
  <div>
    <div class="mx-md-15 mx-5 mt-10 pb-10" style="padding-top: 8%">
      <div
        class="
          text-h2 text-md-h1
          font-weight-bold
          text-center
          letter-spacing-10
        "
        style="line-height: 1.2"
      >
        Credit Data for DeFi
        <div class="text-h3 font font-weight-medium harmony mt-2">
          on Harmony Testnet
        </div>
      </div>
    </div>

    <div class="ma-md-15 py-5">
      <div class="d-flex justify-center align-bottom text-center mx-5 mx-md-15">
        <v-text-field v-model="address" label="Address (0x)" outlined rounded>
        </v-text-field>

        <v-btn
          dark
          class="mx-4"
          fab
          elevation="0"
          color="yellow darken-1"
          :disabled="loading"
          @click="requestData()"
        >
          <v-icon> mdi-flash </v-icon>
        </v-btn>
        <!-- <div>
        <div class="text-h4 orange--text" style="line-height: 1.4">We're sorry, Demo is closed for now!</div>
        <div class="my-5">Join our <a href="https://discord.gg/TaHXKnrgkb">Discord</a> for updates</div>
        </div> -->
      </div>
    </div>

    <div v-if="!loading" class="pb-10">
      <!-- Valuation -->
      <div v-if="valuation">
        <div class="text-h4 font-weight-bold mx-15 px-10 mb-10">
          Total Valuation
          <div class="text-h3 my-4">$ {{ valuation.total.toFixed(2) }}</div>
        </div>

        <!-- <div>Assets</div> -->

        <v-slide-group multiple show-arrows class="mx-md-15">
          <v-slide-item
            v-for="asset in valuation.items"
            :key="asset.contract_address"
          >
            <v-card class="ma-5" flat>
              <div class="d-md-flex">
                <v-img :src="asset.logo_url" width="120px" contain></v-img>
                <div>
                  <v-card-title>{{ asset.contract_name }}</v-card-title>
                  <v-card-subtitle>$ {{ asset.quote }}</v-card-subtitle>
                  <v-card-text>
                    <div>
                      Balance:
                      {{
                        parseFloat(asset.balance) /
                        10 ** parseInt(asset.contract_decimals)
                      }}
                    </div>
                    <div>
                      Contract Address:
                      <div>{{ asset.contract_address }}</div>
                    </div>
                    <div>Price: $ {{ asset.quote_rate }}</div>
                  </v-card-text>
                </div>
              </div>
            </v-card>
          </v-slide-item>
        </v-slide-group>
      </div>

      <!-- Credit Data -->
      <div v-if="credit" class="mx-md-15 mx-5 px-10 mb-5 mt-15">
        <div class="text-h4 font-weight-bold pt-10">Credit Data</div>

        <div class="mt-15 justify-space-around">
          <div class="text-h4 my-5">
            Current Debt
            <div class="text-h3 font-weight-bold">
              $ {{ credit.current_borrowed.toFixed(2) }}
            </div>
          </div>
          <div class="d-flex flex-wrap justify-space-around">
            <div class="text-h5 py-10">
              Total Debt History
              <div class="text-h4">
                $ {{ credit.total_borrowed.toFixed(2) }}
              </div>
            </div>

            <div class="text-h5 py-10">
              Total Repaid History
              <div class="text-h4">$ {{ credit.total_repaid.toFixed(2) }}</div>
            </div>

            <v-progress-linear
              class="mt-2"
              style="border-radius: 100px"
              background-color="yellow darken-1"
              color="success"
              height="50"
              reverse
              :value="(credit.total_repaid / credit.total_borrowed) * 100"
            ></v-progress-linear>
          </div>

          <div class="text-h4 my-5 mt-15">
            Current Supplied
            <div class="text-h3 font-weight-bold my-4 mb-10">
              $ {{ credit.current_supplied.toFixed(2) }}
            </div>
          </div>
          <div class="d-flex flex-wrap justify-space-around">
            <div class="text-h5 py-10">
              Total Supplied History
              <div class="text-h4">
                $ {{ credit.total_supplied.toFixed(2) }}
              </div>
            </div>

            <div class="text-h5 py-10">
              Total Redeemed History
              <div class="text-h4">
                $ {{ credit.total_redeemed.toFixed(2) }}
              </div>
            </div>

            <v-progress-linear
              class="mt-2"
              style="border-radius: 100px"
              background-color="yellow darken-1"
              color="success"
              height="50"
              reverse
              :value="(credit.total_redeemed / credit.total_supplied) * 100"
            ></v-progress-linear>
          </div>
        </div>

        <v-slide-group multiple show-arrows>
          <v-slide-item> </v-slide-item>
        </v-slide-group>
      </div>

      <!-- Scores -->
      <div v-if="score != null" class="mx-md-15 mx-5 px-10 mb-5 mt-15">
        <div class="text-h4 font-weight-bold pt-10">Credit Score</div>
        <div class="d-flex text-center justify-center ma-5">
          <div>
            <div class="text-h4">Overall Score</div>

            <v-progress-circular
              :rotate="90"
              :size="400"
              :width="80"
              :value="score"
              color="yellow darken-2"
              class="ma-5"
            >
              <div class="text-h3">{{ score }}</div>
            </v-progress-circular>
          </div>
        </div>
        <div class="d-flex flex-wrap justify-space-around text-center">
          <div class="ma-5">
            <div class="text-h5">Supply Score</div>

            <v-progress-circular
              :rotate="90"
              :size="250"
              :width="50"
              :value="supply_score"
              color="yellow darken-2"
              class="ma-5"
            >
              <div class="text-h4">{{ supply_score }}</div>
            </v-progress-circular>
          </div>
          <div class="ma-5">
            <div class="text-h5">Value Score</div>

            <v-progress-circular
              :rotate="90"
              :size="250"
              :width="50"
              :value="value_score"
              color="yellow darken-2"
              class="ma-5"
            >
              <div class="text-h4">{{ value_score }}</div>
            </v-progress-circular>
          </div>
          <div class="ma-5">
            <div class="text-h5">Debt Score</div>

            <v-progress-circular
              :rotate="90"
              :size="250"
              :width="50"
              :value="debt_score"
              color="yellow darken-2"
              class="ma-5"
            >
              <div class="text-h4">{{ debt_score }}</div>
            </v-progress-circular>
          </div>
          <div class="ma-5">
            <div class="text-h5">Repayment Score</div>

            <v-progress-circular
              :rotate="90"
              :size="250"
              :width="50"
              :value="repayment_score"
              color="yellow darken-2"
              class="ma-5"
            >
              <div class="text-h4">{{ repayment_score }}</div>
            </v-progress-circular>
          </div>
        </div>
      </div>
    </div>

    <div v-else class="d-flex justify-center">
      <div>
        <v-progress-circular
          size="150"
          width="10"
          color="yellow darken-2"
          indeterminate
        >
        </v-progress-circular>
        <div class="text-body-2 text-center my-5" style="color: orange">
          Requesting
        </div>
      </div>
      {{ error }}
    </div>
  </div>
</template>

<script>
const axios = require('axios')

export default {
  components: {
    // Web3ModalVue,
  },
  data() {
    return {
      address: '',
      score: null,
      supply_score: null,
      value_score: null,
      repayment_score: null,
      debt_score: null,
      valuation: null,

      credit: null,

      error: null,
      loading: false,
    }
  },

  methods: {
    requestData() {
      this.score = null
      this.credit = null
      this.valuation = null
      this.loading = true

      try {
        const scoreReq = axios.get(
          `https://api.node0.chainscore.finance/score/${this.address}`
        )

        const valueReq = axios.get(
          `https://api.node0.chainscore.finance/value/total/${this.address}`
        )

        const creditReq = axios.get(
          `https://api.node0.chainscore.finance/credit/getAllPositions/${this.address}`
        )

        axios.all([scoreReq, valueReq, creditReq]).then(
          axios.spread((...responses) => {
            this.score = parseFloat(responses[0].data.score).toFixed(2) * 100
            this.supply_score =
              parseFloat(responses[0].data.supply_score).toFixed(2) * 100
            this.value_score =
              parseFloat(responses[0].data.value_score).toFixed(2) * 100
            this.repayment_score =
              parseFloat(responses[0].data.repayment_score).toFixed(2) * 100
            this.debt_score =
              parseFloat(responses[0].data.debt_score).toFixed(2) * 100

            this.valuation = responses[1].data
            this.credit = responses[2].data

            this.loading = false
          })
        )
      } catch (err) {
        this.loading = false
        this.error = err
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