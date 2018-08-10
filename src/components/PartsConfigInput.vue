<template>
  <div class="block">
    <strong><slot></slot></strong>
    <div>({{ min }}~{{ max }})</div>
    <el-slider 
      :value="value"
      :min="min"
      :max="max"
      :step="step"
      show-input
      show-input-controls
      input-size="mini"
      @change="handleChange"
      @input="handleChange"
    ></el-slider>
  </div>
</template>


<script>
export default {
  name: "parts-config-input",

  props: {
    value: Number,
    min: Number,
    max: Number,
    step: Number
  },
  methods: {
    handleChange(val) {
      this.$emit("input", val);
    }
  }
};
</script>

<style scoped>
.block {
  margin-bottom: 1rem;
}
</style>