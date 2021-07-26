<template>
  <h1 class="title">Calculator App with Vue3</h1>
  <div class="container">
    <div class="row">
      <calculator-part class="col-sm-5 col-12  mt-2"></calculator-part>
      <calculator-history class="col-sm-5 col-12 mt-6"></calculator-history>
    </div>
  </div>
</template>

<script>
import CalculatorPart from "./components/CalculatorPart.vue";
import CalculatorHistory from "./components/CalculatorHistory.vue";
export default {
  components: {
    CalculatorPart,
    CalculatorHistory,
  },
  data() {
    return {
      storeHistory:[],
    }
  },
  provide() {
    return {
      storeHistory: this.storeHistory,
    };
  },
};
</script>

<style lang="scss">
html {
  margin: 0;
  background-color: rgba(31, 41, 55, $alpha: 1);
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 1rem;
    background-color: rgba(31, 41, 55, $alpha: 1);
    .title {
      color: rgb(82, 174, 228);
    }
  }
}
</style>
