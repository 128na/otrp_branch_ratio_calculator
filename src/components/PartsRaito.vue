<template>
  <h3 :class="{ superiority: isSuperiority, inferiority: isInferiority }">{{ value }}%</h3>
</template>

<script>
export default {
  name: "parts-raito",
  props: {
    value: Number
  },
  computed: {
    isSuperiority() {
      return this.value >= 75;
    },
    isInferiority() {
      return this.value <= 25;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.superiority {
  color: #198219;
}
.inferiority {
  color: #c71b1b;
}
h3 {
  margin-top: 1rem;
}
</style>
