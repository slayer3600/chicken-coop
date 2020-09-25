<template>
  <v-card class="mx-auto text-center" max-width="400" :loading="loading">
    <v-list-item two-line>
      <v-list-item-content>
        <v-list-item-title class="headline">Huntsman Chicken Coop</v-list-item-title>
        <v-list-item-subtitle>Updated: {{latestUpdate}}</v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>

    <v-card-text>
      <v-row align="center">
        <v-col class="display-3 text-center">
          {{chickenCoopTemp}}&deg;
        </v-col>
      </v-row>
      <v-row align="center" class="mb-n16">
        <v-col class="display-3 text-center">
          <vue-speedometer
          :value="chickenCoopTemp"
          :minValue="0"
          :maxValue="120"
          :maxSegmentLabels="12"
          :segments="12"
          needleColor='red'
          currentValueText="" />
        </v-col>
      </v-row>
    </v-card-text>

    <!-- <v-btn elevation="2" v-on:click="getTempurature">Test Web Service</v-btn> -->
    <!-- <div>
      {{ambientWeatherResults}}
    </div> -->
   </v-card>
</template>

<script>

import axios from 'axios';
import VueSpeedometer from 'vue-speedometer';
import moment from 'moment';

export default {

  components: {
    VueSpeedometer,
  },

  data: () => ({
    ambientWeatherMACAddress: process.env.VUE_APP_MAC_ADDRESS,
    ambientWeatherResults: null,
    chickenCoopTemp: null,
    timer: '',
    latestUpdate: null,
    refreshRateInSeconds: 10,
    loading: true,
    apiKey: process.env.VUE_APP_API_KEY,
    deviceKey: process.env.VUE_APP_DEVICE_KEY,
  }),

  methods: {
    async getTempurature() {
      this.loading = true;
      const results = await axios.get(`https://api.ambientweather.net/v1/devices/${this.ambientWeatherMACAddress}?apiKey=${this.deviceKey}&applicationKey=${this.apiKey}&limit=1`);
      // eslint-disable-next-line prefer-destructuring
      this.ambientWeatherResults = results.data[0];
      // eslint-disable-next-line prefer-destructuring
      this.chickenCoopTemp = results.data[0].temp1f;
      this.latestUpdate = moment(new Date()).format('MM/DD/YYYY, h:mm:ss a');
      this.loading = false;
    },
  },
  created() {
    this.getTempurature();
    this.timer = setInterval(this.getTempurature, this.refreshRateInSeconds * 1000);
  },
  beforeDestroy() {
    clearInterval(this.timer);
  },

};
</script>

<style>

</style>
