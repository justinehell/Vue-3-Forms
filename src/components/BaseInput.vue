<template>
  <label>{{ label }}</label>
  <input
    v-bind="$attrs"
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
    :placeholder="label"
    class="field"
  />
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      default: '',
    },
    modelValue: {
      type: [String, Number],
      defualt: '',
    },
  },
};
</script>

<style></style>
