<template>
  <div class="custom-select" :tabindex="tabindex" @blur="open = false">
    <div class="selected" :class="{ open: open }" @click="open = !open">
      {{ pcurrency }}
    </div>
    <div class="items" :class="{ selectHide: !open }">
      <div
        v-for="(option, i) of options"
        :key="i"
        @click="
          selected = option.currency;
          open = false;
          $emit('optionSelect', option);
          $emit('updateSelection');
        "
      >
        {{ option.currency }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  emits: ["optionSelect", "updateSelection"],
  props: {
    pcurrency: {
      type: String,
      requered: true,
    },
    options: {
      type: Array,
      required: true,
    },
    default: {
      type: String,
      required: false,
      default: null,
    },
    pselected: {
      type: String,
      required: false,
      default: null,
    },
    tabindex: {
      type: Number,
      required: false,
      default: 0,
    },
  },
  data() {
    return {
      selected: this.default
        ? this.default
        : this.options.length > 0
        ? this.options[0]
        : null,
      open: false,
    };
  },
};
</script>

<style lang="scss" scoped>
@import "../scss/variables.scss";

.custom-select {
  position: relative;
  width: 100%;
  text-align: left;
  outline: none;
  height: 47px;
  line-height: 47px;
}

.custom-select .selected {
  background-color: #ffffff;
  border-radius: 6px;
  border: 1px solid #ffffff;
  color: rgb(0, 0, 0);
  padding-left: 1em;
  cursor: pointer;
  user-select: none;
}

.custom-select .selected.open {
  border: 1px solid $selector-primary-color;
  border-radius: 6px 6px 0px 0px;
}

.custom-select .selected:after {
  position: absolute;
  content: "";
  top: 22px;
  right: 1em;
  width: 0;
  height: 0;
  border: 5px solid transparent;
  border-color: $selector-primary-color transparent transparent transparent;
}

.custom-select .items {
  color: rgb(0, 0, 0);
  border-radius: 0px 0px 6px 6px;
  overflow: hidden;
  border-right: 1px solid $selector-primary-color;
  border-left: 1px solid $selector-primary-color;
  border-bottom: 1px solid $selector-primary-color;
  position: absolute;
  background-color: #ffffff;
  left: 0;
  right: 0;
  z-index: 1;
}

.custom-select .items div {
  color: rgb(0, 0, 0);
  padding-left: 1em;
  cursor: pointer;
  user-select: none;
}

.custom-select .items div:hover {
  background-color: $selector-primary-color;
}

.selectHide {
  display: none;
}
</style>
