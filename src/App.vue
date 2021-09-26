<template>
  <calculator />
</template>

<script>
import Calculator from "./components/Calculator.vue";

export default {
  name: "App",
  components: {
    Calculator,
  },
};
</script>

<style lang="scss">
body {
  background-color: #ccab52;
  background-image: linear-gradient(to left, #ccab52, #d9d2bd);
}
</style>
