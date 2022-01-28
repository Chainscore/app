<template>
  <div>
    <div class="mx-md-15 mx-5 mt-15 pt-15">
      <div
        class="text-h2 text-md-h1 font-weight-bold text-center letter-spacing-10"
        style="line-height: 1.2"
      >
        
        <div class="text-md-h1 text-h2 font font-weight-medium harmony mt-2">
          Credit Analytics for DeFi
        </div>
        
        </div>
        <div class="mt-10">
        <div class="text--disabled text-center">
          ✔️ Data supported from Ethereum Mainnet
        </div>
        <div class="text--disabled text-center">
          ✔️ ERC20 Assets and NFTs for valuation
        </div>
        <div class="text--disabled text-center">
          ✔️ Credit History (Lending/Borrowing) tracked from Aave V2 and
          Compound
        </div>
      </div>
    </div>

    <div class="ma-md-15 pt-md-0 pt-10 px-md-15">
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

    <!-- <div v-if="!loading" class="pb-10"> -->
    <div v-if="true" class="pb-10">

      <!-- Valuation -->
      <div v-if="valuation">

        <div class="text-md-h3 text-h4 harmony font-weight-bold mx-md-15 mx-10 px-md-10 mb-10">Valuation</div>

        <div class="text-md-h4 text-h5 mx-md-15 mx-10 px-md-10 mb-10">
          Total
          <div class="text-md-h3 text-h4 my-4">$ {{ valuation.total.toFixed(2) }}</div>
        </div>

        <!-- <div>Assets</div> -->

        <v-slide-group multiple show-arrows>
          <v-slide-item
            v-for="asset in valuation.items"
            :key="asset.contract_address"
          >
            <v-card class="ma-md-5 pa-5 mr-5" flat>
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
      <div v-if="credit" class="mx-md-15 mx-md-5 px-10 mb-5 mt-15">
        <div class="text-md-h3 text-h4 harmony font-weight-bold pt-10">Credit Data</div>

        <div class="mt-md-15 mt-10 justify-space-around">
          <div class="text-md-h4 text-h5 my-5">
            Current Debt
            <div class="text-md-h3 text-h4 font-weight-bold">
              $ {{ Math.abs(credit.current_borrowed.toFixed(2)) }}
            </div>
          </div>
          <div class="d-flex flex-wrap justify-space-around">
            <div class="text-h5 py-md-10 py-5 text-md-center">
              Total Debt History
              <div class="text-md-h4 text-h5">
                $ {{ credit.total_borrowed.toFixed(2) }}
              </div>
            </div>

            <div class="text-h5 py-10 text-md-center">
              Total Repaid History
              <div class="text-md-h4 text-h5">$ {{ credit.total_repaid.toFixed(2) }}</div>
            </div>

            <v-progress-linear
              class="mt-2"
              style="border-radius: 10px"
              background-color="yellow darken-1"
              color="success"
              height="40"
              reverse
              :value="(credit.current_borrowed / credit.total_borrowed) * 100"
            ></v-progress-linear>
          </div>

          <div class="text-md-h4 text-h5 my-5 mt-15">
            Current Supplied
            <div class="text-md-h3 text-h4 font-weight-bold my-4 mb-10">
              $ {{ Math.abs(credit.current_supplied.toFixed(2)) }}
            </div>
          </div>
          <div class="d-flex flex-wrap justify-space-around">
            <div class="text-h5 py-10">
              Total Supplied History
              <div class="text-md-h4 text-h5">
                $ {{ credit.total_supplied.toFixed(2) }}
              </div>
            </div>

            <div class="text-h5 py-10">
              Total Redeemed History
              <div class="text-md-h4 text-h5">
                $ {{ credit.total_redeemed.toFixed(2) }}
              </div>
            </div>

            <v-progress-linear
              class="mt-2"
              style="border-radius: 10px"
              background-color="yellow darken-1"
              color="grey"
              height="40"
              reverse
              :value="(credit.current_supplied / credit.total_supplied) * 100"
            ></v-progress-linear>
          </div>
        </div>

        <v-slide-group multiple show-arrows>
          <v-slide-item> </v-slide-item>
        </v-slide-group>
      </div>

      <!-- Scores -->
      <div v-if="score != null" class="mx-md-15 mx-5 px-md-10 px-5 mb-5 mt-15">
        <div class="text-md-h3 text-h4 harmony font-weight-bold pt-10">Credit Score</div>
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
    </div>

    <div v-else class="d-flex justify-center my-5">
      <div>
        <v-progress-circular
          size="150"
          width="10"
          color="yellow darken-2"
          indeterminate
        >
        </v-progress-circular>
      </div>
    </div>
    {{ error }}
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

      axios
        .get(`https://api.node0.chainscore.finance/score/${this.address}`)
        .then((resp) => {
          this.score = parseFloat(resp.data.score).toFixed(2) * 100
          this.supply_score =
            parseFloat(resp.data.supply_score).toFixed(2) * 100
          this.value_score = parseFloat(resp.data.value_score).toFixed(2) * 100
          this.repayment_score =
            parseFloat(resp.data.repayment_score).toFixed(2) * 100
          this.debt_score = parseFloat(resp.data.debt_score).toFixed(2) * 100

          this.valuation = resp.data.value
          this.credit = resp.data.credit

          this.loading = false
        })

        .catch((err) => {
          this.loading = false
          this.error = err
        })
    },
  },
}
</script>

<style lang="css">
.harmony {
  background: -webkit-linear-gradient(right, #c5d81c, #ede793);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>
