<template>
  <div class="container">
    <calculator-app />
  </div>
</template>

<script>
import CalculatorApp from '@/components/CalculatorApp.vue'

export default {
  name: 'App',
  components: {
    CalculatorApp
  }
}
</script>

<style lang="scss">
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  .container {
    padding: 0 20px;
    height: 100vh;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    text-align: center;
    color: #2c3e50;
  }
</style>
