<template>
  <div class="calculator">
    <div class="display">{{ display }}</div>
    <div class="buttons-grid">
      <button v-for="(btn, i) in buttons" :key="i" @click="onButtonClick(btn)" :class="btn.class">{{ btn.label
        }}</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      display: '0',
      buttons: [
        { label: '7' },
        { label: '8' },
        { label: '9' },
        { label: '+', class: 'op' },
        { label: '4' },
        { label: '5' },
        { label: '6' },
        { label: '-', class: 'op' },
        { label: '1' },
        { label: '2' },
        { label: '3' },
        { label: '×', class: 'op' },
        { label: '0' },
        { label: '.' },
        { label: 'C', class: 'ctrl' },
        { label: '÷', class: 'op' },
        { label: '=', class: 'equal' }
      ],
      previous: '',
      operator: null,
      waitingForOperand: false
    }
  },
  methods: {
    onButtonClick(btn) {
      const val = btn.label;
      if (!isNaN(val) || val === '.') {
        this.appendNumber(val);
      } else if (['+', '-', '×', '÷'].includes(val)) {
        this.chooseOperator(val);
      } else if (val === 'C') {
        this.clear();
      } else if (val === '=') {
        this.calculate();
      }
    },
    appendNumber(num) {
      if (this.waitingForOperand) {
        this.display = num === '.' ? '0.' : num;
        this.waitingForOperand = false;
      } else if (this.display === '0' && num !== '.') {
        this.display = num;
      } else if (num === '.' && this.display.includes('.')) {
        return;
      } else {
        this.display += num;
      }
    },
    chooseOperator(op) {
      if (this.operator && !this.waitingForOperand) {
        this.calculate();
      }
      this.operator = op;
      this.previous = this.display;
      this.waitingForOperand = true;
    },
    clear() {
      this.display = '0';
      this.previous = '';
      this.operator = null;
      this.waitingForOperand = false;
    },
    calculate() {
      let result = 0;
      const prev = parseFloat(this.previous);
      const curr = parseFloat(this.display);
      if (isNaN(prev) || isNaN(curr) || !this.operator) return;
      switch (this.operator) {
        case '+':
          result = prev + curr;
          break;
        case '-':
          result = prev - curr;
          break;
        case '×':
          result = prev * curr;
          break;
        case '÷':
          result = curr === 0 ? '错误' : prev / curr;
          break;
        default:
          return;
      }
      this.display = String(result);
      this.previous = '';
      this.operator = null;
      this.waitingForOperand = false;
    }
  }
}
</script>

<style scoped>
.calculator {
  width: 280px;
  margin: 40px auto;
  padding: 20px;
  background: #e6f9ec;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(66, 185, 131, 0.1);
}

.display {
  background: #d0f5e2;
  color: #2e7d4f;
  font-size: 2em;
  text-align: right;
  padding: 12px;
  border-radius: 6px;
  margin-bottom: 16px;
  min-height: 40px;
  letter-spacing: 1px;
}

.buttons-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

button {
  padding: 18px 0;
  font-size: 1.1em;
  border: none;
  border-radius: 6px;
  background: #b2e9c7;
  color: #2e7d4f;
  cursor: pointer;
  transition: background 0.2s;
}

button.op {
  background: #a0e0bb;
  color: #1b5e20;
  font-weight: bold;
}

button.ctrl {
  background: #e0f7fa;
  color: #00897b;
}

button.equal {
  grid-column: span 4;
  background: #42b983;
  color: #fff;
  font-weight: bold;
  font-size: 1.2em;
}

button:hover {
  background: #a0e0bb;
}
</style>
