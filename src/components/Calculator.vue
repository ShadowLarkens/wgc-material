<template>
  <v-container>
    <v-layout text-xs-center wrap>
      <v-flex xs12>
<!--         <v-img
          :src="require('../assets/logo.svg')"
          class="my-3"
          contain
          height="200"
        ></v-img> -->
      </v-flex>

      <v-flex xs12>
        <h1 class="display-2 font-weight-bold mb-3">
        </h1>
      </v-flex>

      <v-flex xs12 md5>
        <h3 class="font-weight-normal display-1">Generator</h3>
        <v-text-field mask="#######" label="Prey Weight (lb)" placeholder="140" v-model="preyWeight"></v-text-field>
        <v-text-field mask="#######" label="Pred Weight (lb)" placeholder="140" v-model="predWeight"></v-text-field>
        <v-divider></v-divider>
        <v-subheader>Prey Fat%</v-subheader>
        <v-slider v-model="preyFat" step="1" :max="4" ticks="always" tick-size="2" :tick-labels="preyFatLabels"></v-slider>
        <v-divider></v-divider>
        <v-btn block color="primary" @click="generateData()">Check Gains!</v-btn>
      </v-flex>

      <v-spacer></v-spacer>

      <v-flex xs12 md6>
        <!-- <div v-if="preyAdipose">
          <div>Prey's Weight: {{preyWeight}} lb</div>
          <div>Pred's Original Weight: {{predWeight}} lb</div>
          <div>Calculated nutritional value of prey: {{preyKCal}} KCal</div>
          <div>Absorbed weight: {{absorbedWeight}} lb</div>
          <div>Pred's End Weight: {{predEndWeight}} lb</div>
          <div>Waste Weight: {{wasteWeight}} lb</div>
        </div> -->
        <v-card v-if="preyAdipose">
          <v-card-text>
            <div class="text-xs-left display-3">Results</div>
            <!-- <div class="text-xs-left">Prey's Weight: {{preyWeight | round}} lb</div>
            <div class="text-xs-left">Pred's Original Weight: {{predWeight | round}} lb</div>
            <div class="text-xs-left">Calculated nutritional value of prey: {{preyKCal | round}} KCal</div>
            <div class="text-xs-left">Absorbed weight: {{absorbedWeight | round}} lb</div>
            <div class="text-xs-left">Pred's End Weight: {{predEndWeight | round}} lb</div>
            <div class="text-xs-left">Waste Weight: {{wasteWeight | round}} lb</div> -->
            <v-list dense>
              <v-list-tile>
                <v-list-tile-content class="subheader">Prey Weight:</v-list-tile-content>
                <v-list-tile-content class="align-end title">{{preyWeight | round}} lb</v-list-tile-content>
              </v-list-tile>
              <v-list-tile>
                <v-list-tile-content class="subheader">Prey Adipose:</v-list-tile-content>
                <v-list-tile-content class="align-end title">{{preyAdipose | round}} lb</v-list-tile-content>
              </v-list-tile>
              <v-list-tile>
                <v-list-tile-content class="subheader">Prey Calories:</v-list-tile-content>
                <v-list-tile-content class="align-end title">{{preyKCal | round}} kcal</v-list-tile-content>
              </v-list-tile>
              <v-list-tile>
                <v-list-tile-content class="subheader">Pred's Starting Weight:</v-list-tile-content>
                <v-list-tile-content class="align-end title">{{predWeight | round}} lb</v-list-tile-content>
              </v-list-tile>
              <v-list-tile>
                <v-list-tile-content class="subheader">Absorbed Weight:</v-list-tile-content>
                <v-list-tile-content class="align-end title">{{absorbedWeight | round}} lb</v-list-tile-content>
              </v-list-tile>
              <v-list-tile>
                <v-list-tile-content class="subheader">Pred's End Weight:</v-list-tile-content>
                <v-list-tile-content class="align-end title">{{predEndWeight | round}} lb</v-list-tile-content>
              </v-list-tile>
              <v-list-tile>
                <v-list-tile-content class="subheader">Waste material:</v-list-tile-content>
                <v-list-tile-content class="align-end title">{{wasteWeight | round}} lb</v-list-tile-content>
              </v-list-tile>
            </v-list>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>

const ratio = {
  "WeightToAdipose": (8.72 / 65.99),
  "AdiposeToKCal": (49938.50 / 8.72) * 0.45359237,
  "BaseKCal": 143771.33 - 49938.50,
  "KCalToAdipose": 1 / 3500,
}

const preyFatRatio = [
  0.6,
  0.8,
  1,
  1.2,
  1.4
]

export default {
  data: () => ({
    preyWeight: 140,
    predWeight: 140,
    preyFat: 2,
    preyFatLabels: [
      'Lithe',
      'Thin',
      'Average',
      'Fat',
      'Obese'
    ],

    /* Results */
    preyAdipose: null,
    preyKCal: null,
    absorbedWeight: null,
    predEndWeight: null,
    wasteWeight: null
  }),
  methods: {
    generateData () {
      this.preyAdipose = this.preyWeight * ratio.WeightToAdipose * preyFatRatio[this.preyFat];
      this.preyKCal = ratio.BaseKCal + (this.preyAdipose * ratio.AdiposeToKCal);
      this.absorbedWeight = this.preyKCal * ratio.KCalToAdipose;
      this.predEndWeight = this.predWeight + this.absorbedWeight;
      this.wasteWeight = this.preyWeight * 0.16;
      console.log(this);
    }
  },
  filters: {
    round (value) {
      return value.toFixed(2);
    }
  }
}
</script>

<style>

</style>
