<template>
  <bg-app />
  <div class="calculator">
    <div class="сalculator__result">
      {{ calculatorValue || 0}}
    </div>
    <div class="сalculator__elements">
      <div class="bg-gray element"
           :class="[
             {'bg-gold': ['C','*','/','-','+','%','=','←'].includes(n)},
             {'zero': n === 0}
             ]"
           @click="action(n)"
           v-for="n in calculatorElements" :key="n"
      >
        {{n}}
      </div>
    </div>
  </div>
</template>

<script>
import BgApp from '@/components/BgApp'

export default {
  name: 'CalculatorApp',
  components: {
    BgApp
  },
  data () {
    return {
      calculatorValue: '',
      calculatorElements: ['C', '*', '/', '←', 7, 8, 9, '-', 4, 5, 6, '+', 1, 2, 3, '%', 0, '.', '='],
      operator: null,
      previousCalculatorValue: '',
      result: false
    }
  },
  methods: {
    action (n) {
      if (this.calculatorValue === 'на ноль делить нельзя :)' && n !== 'C') return
      /* Append value */
      if (!isNaN(n) || n === '.') {
        if (!this.result || this.operator) this.calculatorValue += n + ''
        else {
          this.result = false
          this.calculatorValue = n + ''
        }
      }
      /* Clear value */
      if (n === 'C') {
        this.calculatorValue = ''
        this.operator = null
        this.result = false
      }
      /* Clear last symbol */
      if (n === '←') {
        this.calculatorValue = this.calculatorValue.slice(0, -1)
      }
      /* Percentage */
      if (n === '%') {
        if (!this.previousCalculatorValue) return
        this.calculatorValue += n
      }
      /* Operators */
      if (['/', '*', '-', '+'].includes(n)) {
        if (!this.calculatorValue && n !== '-') return
        if (this.calculatorValue === '-') return
        if (this.operator) {
          this.calculatorValue = this.calculatorValue.slice(0, -1) + n
          this.operator = n
          return
        }
        this.operator = n
        this.previousCalculatorValue = this.calculatorValue
        this.calculatorValue += n
      }
      /* Calculate result */
      if (n === '=') {
        if (!this.operator) return
        const arr = this.calculatorValue.split(this.operator)
        this.calculatorValue = arr[arr.length - 1]
        if (this.calculatorValue.slice(-1) === '%') {
          this.calculatorValue = Number(this.previousCalculatorValue) * Number(this.calculatorValue.slice(0, -1)) / 100
        }
        if (this.operator === '/' && this.calculatorValue === '0') {
          this.calculatorValue = 'на ноль делить нельзя :)'
          return
        }
        this.calculatorValue = this.applyOperator(this.operator, Number(this.previousCalculatorValue), Number(this.calculatorValue)).toString()
        this.result = true
        this.previousCalculatorValue = ''
        this.operator = null
      }
    },
    applyOperator (op, a, b) {
      switch (op) {
        case '+': return a + b
        case '-': return a - b
        case '*': return a * b
        case '/': return a / b
        default: throw Error(`unsupported operator: ${op}`)
      }
    }
  },
  watch: {
    calculatorValue (value) {
      if (!value || value === '-') this.operator = null
    }
  }
}
</script>

<style scoped>
  .calculator {
    color: #ffffff;
    max-width: 350px;
    margin: 20vh auto;
    padding: 30px;
    border-radius: 10px;
    background: #8e8e8e;
    box-shadow:
      inset rgba(0,0,0,.6) 0 -3px 8px,
      inset rgba(252,255,255,.7) 0 3px 8px,
      rgba(0,0,0,.8) 0 3px 8px -3px;
  }
  .сalculator__result {
    background-color: #e2e2e2;
    color: black;
    border-radius: 10px;
    margin-bottom: 40px;
    min-height: 20px;
    display: flex;
    align-items: center;
    justify-content: end;
    padding: 20px;
    word-break: break-all;
  }
  .сalculator__elements {
    display: grid;
    gap: 10px;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(5, 60px);
  }
  .element {
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
    box-shadow:
      inset rgba(0,0,0,.6) 0 -3px 8px,
      inset rgba(252,255,255,.7) 0 3px 8px,
      rgba(0,0,0,.8) 0 3px 8px -3px;
  }
  .element:active {
    transform: translateY(3px)
  }
  .bg-gray {
    background: #bbbbbb;
  }
  .bg-gray:hover {
    cursor: pointer;
    background: #b4b4b4;
  }
  .bg-gold {
    background: #d7ae45;
  }
  .bg-gold:hover {
    background: #f8cb4f;
  }
  .zero {
    grid-column: span 2;
  }
</style>
