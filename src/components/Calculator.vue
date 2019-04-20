<template>
  <div class="overall">
    <h1 class="app-header">{{ msg }}</h1>
    <div class="title-bar">
      <div class="dots">
        <span class="dot red"></span>
        <span class="dot yellow"></span>
        <span class="dot green"></span>
      </div>
    </div>
    <div class="calculator fancify">
      <div class="result">{{ current|| 0 }}</div>
      <div @click="clear" class="key modifier-key">AC</div>
      <div @click="negate" class="key modifier-key">±</div>
      <div @click="percentage" class="key modifier-key">%</div>
      <div @click="divide" class="key operator">÷</div>
      <div @click="append('7')" class="key number-key">7</div>
      <div @click="append('8')" class="key number-key">8</div>
      <div @click="append('9')" class="key number-key">9</div>
      <div @click="multiply" class="key operator">x</div>
      <div @click="append('4')" class="key number-key">4</div>
      <div @click="append('5')" class="key number-key">5</div>
      <div @click="append('6')" class="key number-key">6</div>
      <div @click="subtract" class="key operator">-</div>
      <div @click="append('1')" class="key number-key">1</div>
      <div @click="append('2')" class="key number-key">2</div>
      <div @click="append('3')" class="key number-key">3</div>
      <div @click="add" class="key operator">+</div>
      <div @click="appendZero()" class="key number-key zero">0</div>
      <div @click="addDecimal()" class="key number-key">.</div>
      <div @click="calculate" class="key operator">=</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data () {
    return {
      msg: 'Vue.js Calculator App',
      previous: null,
      current: '',
      operator: null,
    }
  },
  methods: {
    clear() {
      this.current = '';
      this.previous = null;
      this.operator = null;
      this.operatorClicked = false;
    },
    negate() {
      this.current = this.current.charAt(0) === '-' 
        ? this.current.slice(1)
        : `-${this.current}`;
    },
    percentage() {
      this.current = `${parseFloat(this.current) / 100}`;
    },
    append(keyPress) {
      if (this.operatorClicked) {
        this.current = '';
        this.operatorClicked = false;
      }
      this.current = `${this.current}${keyPress}`;
    },
    appendZero() {
      if (parseFloat(this.current) !== 0) {
        this.append('0');
      }
    },
    addDecimal() {
      if (this.current.indexOf('.') === -1) {
        this.append('.');
      }
    },
    setPrevious() {
      this.previous = this.current;
      this.operatorClicked = true;
    },
    divide() {
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },
    multiply() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    subtract() {
      this.operator = (a, b) => a - b;
      this.setPrevious();
    },
    add() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    calculate() {
      if (this.operator !== null)
        this.current = `${this.operator(parseFloat(this.previous), parseFloat(this.current))}`;
        this.operatorClicked = false;
        this.previous = null;
        this.operator = null;
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .app-header {
    font-weight: bold;
  }
  .title-bar {
    position: relative;
    top: 6px;
    border-radius: 5px;
    height: 35px;
    background-color: #333;
  }
  .overall {
    width: 360px;
    margin: 0 auto;
  }
  .dots {
    position: relative;
    left: -135px;
    top: 9px;
  }
  .dot {
    width: 18px;
    height: 18px;
    border-radius: 50%;
    display: inline-block;
    margin: 0 3px;
  }
  .red {
    background-color: red;
  }
  .yellow {
    background-color: goldenrod;
  }
  .green {
    background-color: green;
  }
  .fancify {
    border: 5px solid #333;
    border-radius: 5px;
  }  
  .calculator {    
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(40px, auto);
  }
  .operator {
    background-color: orange;
    color: white;
  }
  .zero {
    grid-column: 1 / 3;
  }
  .result {
    background-color: #666;
    font-weight: bold;
    color: white;
    font-size: 50px;
    padding-right: 15px;
    text-align: right;
    grid-column: 1 / 5;
  }
  .key {
    border: 1px solid #777;
    padding: 10px;
    font-size: 25px;
    cursor: pointer;
  }
  .number-key {
    background-color: #eee;
  }
  .modifier-key {
    background-color: #ccc;
  }
</style>
