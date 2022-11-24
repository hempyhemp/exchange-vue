<template>
  <h1 class="label" ref="zxc">Exchanger</h1>
  <div class="content">
    <ui-block
      :Id="1"
      v-model="currencies.block1.value"
      :pcurrency="currencies.block1.currency"
      :rates="rates"
      @selectedRate="onSelectedRate"
    />
    <img class="img" src="/exchange.png" @click="sideChange" />
    <ui-block
      :Id="2"
      v-model="currencies.block2.value"
      :pcurrency="currencies.block2.currency"
      :rates="rates"
      @selectedRate="onSelectedRate"
    />
  </div>
</template>

<script>
import ExchangeCourse from "@/resourses/latest.json";
import UiBlock from "@/components/UiBlock.vue";

export default {
  components: {
    UiBlock,
  },

  data() {
    return {
      api: `https://openexchangerates.org/api/latest.json?app_id=38fa16ca8548411c899a81409168a9cb`,
      rates: [],
      currencies: {
        block1: { value: 1, rate: 1, currency: "USD" },
        block2: { value: 1, rate: 1, currency: "USD" },
      },
    };
  },
  methods: {
    onSelectedRate(activeRate) {
      if (activeRate[0] == 1) {
        this.currencies.block1.rate = activeRate[1].rate;
        this.currencies.block1.currency = activeRate[1].currency;
        let valueToUsd =
          this.currencies.block1.value / this.currencies.block1.rate;
        this.currencies.block2.value = valueToUsd * this.currencies.block2.rate;
      } else {
        this.currencies.block2.rate = activeRate[1].rate;
        this.currencies.block2.currency = activeRate[1].currency;
        let valueToUsd =
          this.currencies.block2.value / this.currencies.block2.rate;
        this.currencies.block1.value = valueToUsd * this.currencies.block1.rate;
      }
    },
    sideChange() {
      let tempBlock = this.currencies.block1;
      this.currencies.block1 = this.currencies.block2;
      this.currencies.block2 = tempBlock;
      console.log("changed");
    },
    getLatestRates() {
      const data = ExchangeCourse;
      for (const [key, value] of Object.entries(data.rates)) {
        this.rates.push({ currency: key, rate: value });
      }

      //     <div v-for="rate in rates" :key="rate.currency">
      //   {{ rate.currency }} : {{ rate.rate }}
      // </div>
      // fetch(this.api)
      //   .then((response) => response.json())
      //   .then((data) => {
      //     for (const [key, value] of Object.entries(data.rates)) {
      //       this.rates.push({ currency: key, rate: value });
      //     }
      //   })
      //   .catch((e) => {
      //     console.error(e);
      //   });
    },
  },

  created() {
    this.getLatestRates();
  },
};
</script>

<style lang="scss" scoped>
@import "./scss/variables.scss";
.content {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 100px;
}

.label {
  display: flex;
  justify-content: center;
  padding: 100px;
}

.img {
  filter: invert(0.4);
  height: $block-size;
  width: $block-size;
}
</style>
