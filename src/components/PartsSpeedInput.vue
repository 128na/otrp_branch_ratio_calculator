<template>
  <div>
    <div><slot></slot></div>
    <el-input-number 
      :value="value"
      :min="1"
      :max="65535"
      :step="10"
      size="mini"
      controls-position="right"
      @change="handleChange"
      @input="handleChange"
    ></el-input-number>
  </div>
</template>

<script>
export default {
  name: "parts-speed-input",

  props: {
    value: Number
  },
  methods: {
    handleChange(val) {
      this.$emit("input", val);
    }
  }
};
</script>