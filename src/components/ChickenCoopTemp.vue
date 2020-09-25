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

const apiKey = 'bd2b895a49164201beb7ed8cb48abfce92e29246b1044e7fbe1fc765ab2d4782';
const deviceKey = 'f75f1b034b1944038649c8fb32f8f4c53ba640d3c15b4bb0a05e6510b26ace32';

export default {

  components: {
    VueSpeedometer,
  },

  data: () => ({
    ambientWeatherMACAddress: '24:7D:4D:A3:64:FD',
    ambientWeatherResults: null,
    chickenCoopTemp: null,
    timer: '',
    latestUpdate: null,
    refreshRateInSeconds: 10,
    loading: true,
  }),

  methods: {
    async getTempurature() {
      this.loading = true;
      const results = await axios.get(`https://api.ambientweather.net/v1/devices/${this.ambientWeatherMACAddress}?apiKey=${deviceKey}&applicationKey=${apiKey}&limit=1`);
      // eslint-disable-next-line prefer-destructuring
      this.ambientWeatherResults = results.data[0];
      // eslint-disable-next-line prefer-destructuring
      this.chickenCoopTemp = results.data[0].temp1f;
      this.latestUpdate = moment(new Date()).format('MM/DD/YYYY, h:mm:ss a');
      this.loading = false;
    },
    getAlert() {
      alert('hello chicken coop');
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
