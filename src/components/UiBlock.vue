<template>
  <div class="block">
    <!-- <h3>cur: {{ activeRate.currency }} rate:{{ activeRate.rate }}</h3> -->

    <img class="img" :src="imgPath" width="60" height="60" />

    <number-input
      class="mb20"
      :model-value="round(modelValue)"
      @update:modelValue="
        $emit('update:modelValue', $event);
        $emit('selectedRate', [Id, activeRate]);
      "
    />

    <custom-select
      :options="this.rates"
      :default="this.rates[149].currency"
      :pcurrency="pcurrency"
      @option-select="onChangeCurrency"
      @update-selection="$emit('selectedRate', [Id, activeRate])"
    />
  </div>
</template>

<script>
import NumberInput from "./NumberInput.vue";
import CustomSelect from "./CustomSelect.vue";

export default {
  components: {
    NumberInput,
    CustomSelect,
  },
  name: "UiBlock",
  emits: ["update:modelValue", "selectedRate"],
  props: {
    Id: {
      type: Number,
      required: true,
    },
    modelValue: {
      type: Number,
      required: true,
    },
    rates: {
      type: Array,
      required: true,
    },
    pcurrency: {
      type: String,
      requered: true,
    },
  },
  data() {
    return {
      activeCurrency: this.rates[149].currency,
    };
  },
  computed: {
    activeRate() {
      return (
        this.rates.find((rate) => rate.currency === this.activeCurrency) ??
        this.rates[0]
      );
    },
    imgPath() {
      return `${import.meta.env.BASE_URL}/${this.pcurrency}.png`;
    },
  },
  methods: {
    onChangeCurrency(option) {
      this.activeCurrency = option.currency;
    },
    round(number) {
      return Math.round(parseFloat(number) * 1000) / 1000;
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../scss/variables.scss";

.img {
  display: block;
  position: absolute;
  margin: -75px 0px 0px 205px;
}
.block {
  display: flex;
  flex-direction: column;
  align-content: center;
  justify-content: center;

  border: 1px solid rgb(255, 255, 255);
  background: $block-background-color;
  padding: 20px;
  margin: 0px;
  border-radius: 12px;

  height: $block-height;
  width: $block-width;
}

.mb15 {
  margin-bottom: 15px;
}
.mb20 {
  margin-bottom: 25px;
}
.mb25 {
  margin-bottom: 25px;
}
</style>
