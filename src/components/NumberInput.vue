<template>
  <input
    class="input"
    type="text"
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>

<script>
export default {
  emits: ["update:modelValue"],
  props: {
    modelValue: {
      type: Number,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../scss/variables.scss";

.input {
  appearance: none;
  width: 75%;
  height: 44px;
  line-height: 1.25;
  padding: 12px 12px;

  outline: none;
  border: 1px solid $border-color;
  border-radius: 6px;
  color: $font-color;
  font-size: 1rem;

  &:focus {
    border-color: $primary-color;
  }

  &:hover {
    border-color: $primary-color;
  }

  &.invalid {
    border-color: $error-color;
    color: $error-color;
    &::placeholder {
      color: $error-light-color;
    }
  }

  &::placeholder {
    color: $helper-color;
  }
}
</style>
