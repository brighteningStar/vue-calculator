<template>
  <div class="container">
    <div class="calculator-box">
      <input type="text" class="cal-screen" v-model="numberOnScreen" disabled />
      <div class="cal-keys">
        <div class="top-keys">
          <button @click="reset()" class="start-zero">AC</button>
          <button class="placeholder" disabled>BIG ADD</button>
          <button
            @click="setOperator('/')"
            class="operator"
            :class="currentOperator === '/' ? 'active' : ''"
          >
            /
          </button>
        </div>
        <div class="normal-keys">
          <button @click="setNumber('7')" class="numbers">7</button>
          <button @click="setNumber('8')" class="numbers">8</button>
          <button @click="setNumber('9')" class="numbers">9</button>
          <button
            @click="setOperator('*')"
            class="operator"
            :class="currentOperator === '*' ? 'active' : ''"
          >
            X
          </button>
        </div>
        <div class="normal-keys">
          <button @click="setNumber('4')" class="numbers">4</button>
          <button @click="setNumber('5')" class="numbers">5</button>
          <button @click="setNumber('6')" class="numbers">6</button>
          <button
            @click="setOperator('-')"
            class="operator"
            :class="currentOperator === '-' ? 'active' : ''"
          >
            -
          </button>
        </div>
        <div class="normal-keys">
          <button @click="setNumber('1')" class="numbers">1</button>
          <button @click="setNumber('2')" class="numbers">2</button>
          <button @click="setNumber('3')" class="numbers">3</button>
          <button
            @click="setOperator('+')"
            class="operator"
            :class="currentOperator === '+' ? 'active' : ''"
          >
            +
          </button>
        </div>
        <div class="last-keys">
          <button @click="setNumber('0')" class="numbers">0</button>
          <button @click="setNumber('.')" class="numbers">.</button>
          <button @click="getEqual()" class="operator">=</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  data() {
    return {
      numberOnScreen: "0",
      lastNumberOnScreen: null,
      currentOperator: null,
      previousOperator: null,
      operatorSelected: false,
    };
  },

  methods: {
    setNumber(number) {
      if (
        this.numberOnScreen.includes(".") &&
        number === "." &&
        this.operatorSelected === false
      ) {
        return false;
      }
      if (this.operatorSelected === true) {
        this.lastNumberOnScreen = this.numberOnScreen;
        this.numberOnScreen = "0";
        this.operatorSelected = false;
      }
      this.numberOnScreen === "0"
        ? (this.numberOnScreen = number)
        : (this.numberOnScreen += number);
    },
    reset() {
      this.numberOnScreen = "0";
      this.lastNumberOnScreen = null;
      this.lastNumberOnScreen = null;
      this.resetOperators();
    },
    resetOperators() {
      this.currentOperator = null;
      this.previousOperator = null;
      this.operatorSelected = false;
    },
    setOperator(operator) {
      this.operatorSelected = true;
      if (this.currentOperator !== this.previousOperator) {
        this.previousOperator = this.currentOperator;
      }
      this.currentOperator = operator;
      if (this.lastNumberOnScreen) {
        this.calculateNumber(this.previousOperator);
      }
    },
    getEqual() {
      if (this.currentOperator && this.lastNumberOnScreen) {
        this.calculateNumber(this.currentOperator);
        this.resetOperators();
      }
    },
    calculateNumber(operator) {
      switch (operator) {
        case "+":
          this.numberOnScreen =
            parseFloat(this.lastNumberOnScreen) +
            parseFloat(this.numberOnScreen);
          break;

        case "-":
          this.numberOnScreen =
            parseFloat(this.lastNumberOnScreen) -
            parseFloat(this.numberOnScreen);
          break;

        case "*":
          this.numberOnScreen =
            parseFloat(this.lastNumberOnScreen) *
            parseFloat(this.numberOnScreen);
          break;

        case "/":
          this.numberOnScreen =
            parseFloat(this.lastNumberOnScreen) /
            parseFloat(this.numberOnScreen);
          break;
        default:
          this.numberOnScreen = "0";
      }
      this.numberOnScreen = this.roundNumber().toString();
      this.lastNumberOnScreen = null;
    },
    roundNumber() {
      const num = Number((Math.abs(this.numberOnScreen) * 100).toPrecision(15));
      return (Math.round(num) / 100) * Math.sign(this.numberOnScreen);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.container {
  display: grid;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;

  & .calculator-box {
    align-content: center;
    background: #fff;
    width: 250px;
    height: 250px;

    & .cal-screen {
      height: 60px;
      background: #675e40;
      color: #fff;
      font-size: 2em;
      text-align: right;
      border: none;
      width: 100%;
      padding: 0;

      &:focus {
        border: none;
      }
    }

    & .top-keys {
      display: grid;
      grid-template-columns: 1fr 2fr 1fr;
    }

    & .normal-keys {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
    }

    & .last-keys {
      display: grid;
      grid-template-columns: 2fr 1fr 1fr;
    }
  }

  & .operator {
    background-color: #e38b33;
    color: #2a2a29;
  }
  & .numbers {
    background: #f7f6ef;
    color: #2a2a29;
  }
  & .placeholder {
    background: #76030b;
    color: #f3ecec;
  }
  & .start-zero {
    color: #2a2a29;
    background: #696969;
  }

  & button {
    height: 40px;
    border: 1px solid #0f0f0f;
    cursor: pointer;
  }
  & .active {
    transform: scale(0.98);
    box-shadow: 3px 2px 22px 1px rgba(0, 0, 0, 0.24);
    border: 1px solid #3359c4;
  }

  & button:active {
    @extend .active;
  }
}
</style>
