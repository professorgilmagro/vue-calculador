<template>
  <div class="calculator">
    <Display :value="displayValue"/>
    <Button label="AC" triple @onClick="clearMemory"/>
    <Button label="/" operation @onClick="setOperation"/>
    <Button label="7" @onClick="addDigit"/>
    <Button label="8" @onClick="addDigit"/>
    <Button label="9" @onClick="addDigit"/>
    <Button label="*" operation @onClick="setOperation"/>
    <Button label="4" @onClick="addDigit"/>
    <Button label="5" @onClick="addDigit"/>
    <Button label="6" @onClick="addDigit"/>
    <Button label="-" operation @onClick="setOperation"/>
    <Button label="1" @onClick="addDigit"/>
    <Button label="2" @onClick="addDigit"/>
    <Button label="3" @onClick="addDigit"/>
    <Button label="+" operation @onClick="setOperation"/>
    <Button label="0" double @onClick="addDigit"/>
    <Button label="." @onClick="addDigit"/>
    <Button label="=" operation @onClick="setOperation"/>
  </div>
</template>

<script>
import Button from "../components/Button";
import Display from "../components/Display";
export default {
    data() {
        return {
            displayValue: "0",
            clearDisplay: false,
            operation: null,
            values: [0, 0],
            current: 0
        }
    },
  components: { Button, Display },
  methods: {
    clearMemory() {
        Object.assign(this.$data, this.$options.data())
    },
    setOperation(operation) {
        if(this.current === 0) {
            this.operation = operation
            this.current = 1
            this.clearDisplay = true
            return
        }

        if(operation === '=') {
            this.clearDisplay = true
            this.current = 0
            this.values[0] = this.calculate(this.operation, this.values[0], this.values[1])
            this.displayValue = this.values[0]
            this.operation = null
            return
        }

        this.clearDisplay = false
        this.current = 1
    },
    addDigit(n) {
        if (n === '.' && this.displayValue.includes('.')) {
            return;
        }
        
        const clearDisplay = this.displayValue === "0" || this.clearDisplay;
        this.displayValue = clearDisplay ? n : this.displayValue + n;
        this.clearDisplay = false;

        if(n !== '.') {
            this.values[this.current] = parseFloat(this.displayValue)
        }

    },
    calculate(operation, value1, value2) {
      let result = 0;
      switch (operation) {
        case "+":
          result = value1 + value2;
          break;

        case "-":
          result = value1 - value2;
          break;

        case "/":
          result = value1 / value2;
          break;

        case "*":
          result = value1 * value2;
          break;

        default:
          result = 0;
      }

      return result;
    }
  }
};
</script>

<style>
.calculator {
  height: 320px;
  width: 235px;
  border-radius: 5px;
  overflow: hidden;

  display: grid;
  grid-template-columns: repeat(4, 25%);
  grid-template-rows: 1fr repeat(5, 48px);
}

.display {
  -ms-grid-row-span: 4;
}
</style>
