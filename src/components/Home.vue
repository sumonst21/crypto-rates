<template>
<div class="home">
  <!-- Navbar -->
  <v-toolbar color="blue" flat>
    <v-toolbar-title style="color: white;">
      <img src="/static/logo.png" height="40" style="padding-top: 5px;">
    </v-toolbar-title>
    <v-spacer></v-spacer>
    <div style="color: white;">V0.0.1</div>
  </v-toolbar>
  <!-- main interface -->
    <v-layout row wrap style="margin-top: 25px;">
      <v-flex md3 xs12 style=""> <!-- Prices section -->
        <v-container>
          <span style="color: grey;">Exchange rates:</span>
          <div style="background-color: white; text-align: center; border-radius: 5px; font-size: 20px; margin-bottom: 15px;"> <!-- BTC to USD -->
            1 BTC = {{ btc['data']['rates']['USD'] }} USD
          </div>
          <div style="background-color: white; text-align: center; border-radius: 5px; font-size: 20px; margin-bottom: 15px;"> <!-- ETH to USD -->
            1 ETH = {{ eth['data']['rates']['USD'] }} USD
          </div>
          <div style="background-color: white; text-align: center; border-radius: 5px; font-size: 20px; margin-bottom: 15px;"> <!-- LTC to USD -->
            1 LTC = {{ ltc['data']['rates']['USD'] }} USD
          </div>
          <span style="color: grey;">Converter:</span>
          <div> <!-- USD to BTC & ETH converter -->
            <v-text-field
            flat
            label="Enter an amount in USD"
            solo
            v-model="usd"
          ></v-text-field>
          </div>
          <v-layout>
            <v-flex xs4>
              <!-- empty -->
            </v-flex>
            <v-flex xs4>
              <v-btn depressed v-on:click="convert" color="blue"><div style="color: white;">Convert</div></v-btn>
            </v-flex>
            <v-flex xs4>
              <!-- empty -->
            </v-flex>
          </v-layout>
            <div style="background-color: white; text-align: left; border-radius: 5px; font-size: 20px; margin-top: 30px; padding-left: 10px;">
              Bitcoin : <div style="text-align: right; padding-right: 20px;">{{convertedBtc}} BTC</div>
              <br>
              Ethereum : <div style="text-align: right; padding-right: 20px;">{{convertedEth}} ETH</div>
              <br>
              Litecoin : <div style="text-align: right; padding-right: 20px;">{{convertedLtc}} LTC</div>
          </div>
        </v-container>
      </v-flex>
      <v-flex md9 xs12 style="text-align: center; padding-top: 10%;"> <!-- Graphs section -->
        <div style="font-size: 15px;"><strong>Graph coming soon...</strong></div>
        <br>
        // I can't now because I need a database storing all the crypto values, and I am way too lazy to do that.
      </v-flex>
    </v-layout>
</div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Home',
  data () {
    return {
      btc: '',
      eth: '',
      ltc: '',
      usd: '',
      convertedBtc: '0',
      convertedEth: '0',
      convertedLtc: '0'
    }
  },
  created() {
    // Get BTC
    axios.get('https://api.coinbase.com/v2/exchange-rates?currency=BTC').then(response => {
      this.btc = response.data;
    })
    // Get ETH
    axios.get('https://api.coinbase.com/v2/exchange-rates?currency=ETH').then(response => {
      this.eth = response.data;
    })
    // Get LTC
    axios.get('https://api.coinbase.com/v2/exchange-rates?currency=LTC').then(response => {
      this.ltc = response.data;
    })
  },
  methods : {
    async convert() {
      var usd = parseFloat(this.usd);
      // BTC
      var convertedBtc = usd / parseFloat(this.btc['data']['rates']['USD']);
      this.convertedBtc = convertedBtc.toFixed(3) // 3 is the precision rate, it is adjustable
      // ETH
      var convertedEth = usd / parseFloat(this.eth['data']['rates']['USD']);
      this.convertedEth = convertedEth.toFixed(3) // 3 is the precision rate, it is adjustable
      // LTC
      var convertedLtc = usd / parseFloat(this.ltc['data']['rates']['USD']);
      this.convertedLtc = convertedLtc.toFixed(3) // 3 is the precision rate, it is adjustable
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
