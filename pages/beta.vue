<template>
  <v-card>
    <v-card-title>Estimating from Data</v-card-title>
    <v-card-subtitle>Tossing a coin</v-card-subtitle>
    <v-card-text>
      <v-row>
        <template v-for="(side, idx) in counts">
          <v-col :key="counts[idx].name + 'txt'" cols="4" md="3" sm="2">
            <v-text-field
              v-model="counts[idx].count"
              :label="side.name"
            ></v-text-field>
          </v-col>
          <v-col :key="side.name + 'minus'" cols="1" md="1" sm="1">
            <v-btn @click="dec(idx)"><v-icon>mdi-minus</v-icon></v-btn>
          </v-col>
          <v-col :key="side.name + 'plus'" cols="1" md="1" sm="1">
            <v-btn @click="inc(idx)"><v-icon>mdi-plus</v-icon></v-btn>
          </v-col>
          <v-col :key="side.name + 'gap'" cols="1" md="1" sm="1"> </v-col>
        </template>
      </v-row>
      <v-row>
        <v-col cols="8" md="6" sm="4">
          <D3BarChart
            ref="countsRef"
            :config="countsConfig"
            :datum="counts"
          ></D3BarChart>
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
import { D3BarChart } from 'vue-d3-charts'

export default {
  name: 'CoinTossing',

  components: {
    D3BarChart,
  },

  data() {
    return {
      countsConfig: {
        key: 'name',
        values: ['count'],
      },
      counts: [
        { name: 'Heads', count: 35 },
        { name: 'Tails', count: 49 },
      ],
      distr: simulate(1, 1),
    }
  },

  methods: {
    inc(idx) {
      const itm = { ...this.counts[idx] }
      itm.count += 1
      this.counts.splice(idx, 1, itm)
    },
    dec(idx) {
      const itm = { ...this.counts[idx] }
      itm.count -= 1
      this.counts.splice(idx, 1, itm)
    },
  },
}
</script>
