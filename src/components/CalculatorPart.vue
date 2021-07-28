<template>
  <div id="calculator">
    <a
      href="https://github.com/lcijoyce622/Calculator-App-with-Vue3"
      class="row"
      target="_blank"
    >
      <img class="col-1" src="../assets/github.svg" alt="" />
      <p class="col account">lcijoyce622</p>
    </a>

    <div class="row">
      <div class="screen">
        <h1>{{ result }}</h1>
        <p v-if="operator === '='">{{ result }}</p>
        <p v-else-if="preValue === null || result === 0">請輸入數字開始計算</p>
        <p v-else>{{ preValue }}{{ operator }}{{ value }}</p>
      </div>
    </div>
    <div class="row align-items-center">
      <div class="col-6">
        <button type="button" @click="clearResult" class="btn-danger btn">
          AC
        </button>
      </div>
      <div class="col-3">
        <button type="button" @click="deleteResult" class="btn btn-warning">
          DEL
        </button>
      </div>
      <div class="col-3">
        <button type="button" @click="operatorClick('/')" class="btn btn-light">
          /
        </button>
      </div>
    </div>
    <div class="row align-items-center">
      <div class="col-3" v-for="num in [7, 8, 9]" :key="num">
        <button type="button" @click="numClick(num)" class="btn btn-primary">
          {{ num }}
        </button>
      </div>
      <div class="col-3">
        <button type="button" @click="operatorClick('*')" class="btn btn-light">
          *
        </button>
      </div>
    </div>
    <div class="row align-items-center">
      <div class="col-3" v-for="num in [4, 5, 6]" :key="num">
        <button type="button" @click="numClick(num)" class="btn btn-primary">
          {{ num }}
        </button>
      </div>
      <div class="col-3">
        <button type="button" @click="operatorClick('-')" class="btn btn-light">
          -
        </button>
      </div>
    </div>
    <div class="row align-items-center">
      <div class="col-3" v-for="num in [1, 2, 3]" :key="num">
        <button type="button" @click="numClick(num)" class="btn btn-primary">
          {{ num }}
        </button>
      </div>
      <div class="col-3">
        <button type="button" @click="operatorClick('+')" class="btn btn-light">
          +
        </button>
      </div>
    </div>

    <div class="row align-items-start">
      <div class="col-6" :key="num">
        <button type="button" @click="numClick(0)" class="btn btn-primary">
          0
        </button>
      </div>
      <div class="col-3">
        <button type="button" @click="addDot" class="btn btn-primary">.</button>
      </div>
      <div class="col-3">
        <button type="button" @click="getResult" class="btn btn-light">
          =
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  inject: ["storeHistory"],
  data() {
    return {
      num: 0,
      result: 0,
      operator: "",
      value: null,
      preValue: null,
      isFloat: false,
      floatDigits: 0,
    };
  },
  methods: {
    isOverFlow(){
        this.result.toString().length > 13
        ? (this.result = this.result.toString().slice(0, 12))
        : console.log("沒有超過");
      this.isFloat == true
        ? (this.result = parseFloat(this.result))
        : (this.result = parseInt(this.result));
        
    },
    numClick(num) {
      this.isOverFlow();
      this.calculate(num, this.isFloat, this.operator);
    
    },
    calculateFloat(num, floatDigits, operator) {
      //如果已經有按過小數點
      if (num === 0) {
        //小數點後面按.0xxx的時候
        this.result = this.result.toFixed(floatDigits);
        return;
      }
      this.result += num / Math.pow(10, floatDigits);
      this.result = this.result.toFixed(floatDigits);
      if (operator === "") {
        this.result = parseFloat(this.result);
        this.preValue = this.result;
        return;
      }

      if (operator === "=") {
        this.clearResult();
        this.result = num;
        return;
      }
      if (operator !== "") {
        this.result = parseFloat(this.result);
        this.value = this.result;
        return;
      }
    },
    calculate(num, isFloat, operator) {
      console.log(this.operator);
      if (isFloat) {
        if (operator === "=") {
          //重製operator(計算出答案之後沒清空直接又鍵入新值)
          this.clearResult();
          this.result = num;
          return;
        }
        this.result = parseFloat(this.result);
        this.floatDigits++;
        this.calculateFloat(num, this.floatDigits, operator);
        return;
      }
      if (isFloat === false) {
        if (operator === "") {
          //輸入preValue
          this.result === 0
            ? (this.result = num)
            : (this.result = this.result * 10 + num);
          // 判斷是否為個位數

          this.preValue = this.result; //將顯示數字推入preValue
          return;
        }
        if (operator === "=") {
          this.clearResult();
          this.result = num;
          return;
        }
        if (operator !== "") {
          //放入第二個數值
          this.value === null
            ? (this.result = num)
            : (this.result = this.result * 10 + num);
          // 判斷是否為個位數
          this.value = this.result;
          return;
        }
      }
    },
    operatorClick(operator) {
      this.value = null;
      this.operator = operator;
      this.isFloat = false; //按完運算符重製下一個value的小數點
      this.floatDigits = 0; //重至下一個value的小數點
    },
    setReslutFloatLimit() {
      //找結果的小數位數多的以避免溢位
      const preVal =
        this.preValue !== (this.preValue | 0)
          ? parseInt(this.preValue.toString().split(".")[1].length)
          : 0;
      const val =
        this.value !== (this.value | 0)
          ? parseInt(this.value.toString().split(".")[1].length)
          : 0;

      const count = Math.max(preVal, val);
      this.result = parseFloat(this.result.toFixed(count));
    },
    getResult() {
      
      switch (this.operator) {
        case "+":
          this.result = this.preValue + this.value;
          if (
            this.preValue !== (this.preValue | 0) ||
            this.value !== (this.value | 0)
          ) {
            this.setReslutFloatLimit();
          }
          break;
        case "-":
          this.result = this.preValue - this.value;
          if (
            this.preValue !== (this.preValue | 0) ||
            this.value !== (this.value | 0)
          ) {
            this.setReslutFloatLimit();
          }
          break;
        case "*":
          this.result = this.preValue * this.value;
          break;
        case "/":
          this.result = this.preValue / this.value;
          break;
      }
      this.isOverFlow();
      this.storeHistory.push(
        this.preValue + this.operator + this.value + "=" + this.result
      );
      localStorage.setItem(
        "calculateHistory",
        JSON.stringify(this.storeHistory.slice(-10))
      );
      this.preValue = this.result;
      this.operator = "=";
      
    },
    addDot() {
      this.result += "."; //這裡只有顯示在頁面上的小數點
      this.isFloat = true; //變成字串
    },
    clearResult() {
      //全部重製
      this.result = 0;
      this.preValue = null;
      this.value = null;
      this.operator = "";
      this.isFloat = false;
      this.floatDigits = 0;
    },
    deleteResult() {
      if (this.result === (this.result | 0) && this.result < 10) {
        //個位數按DEL直接歸零
        this.result = 0;
        return;
      }
      //一般狀況按DEL
      if (this.result !== (this.result | 0)) {
        this.floatDigits--;
        this.result = this.result.toString(); //轉string
        if (this.floatDigits === 0) {
          this.isFloat = false;
          this.result = this.result.slice(0, this.result.length - 2);
        } else {
          this.isFloat = true;
          this.result = this.result.slice(0, this.result.length - 1);
        }
        //是小數的話小數的位數floatDigits要跟著減掉
        this.result !== (this.result | 0)
          ? (this.result = parseFloat(this.result))
          : (this.result = parseInt(this.result));
      } else {
        //不是小數直接轉number
        this.isFloat = false;
        this.result = this.result.toString(); //轉string
        this.result = this.result.slice(0, this.result.length - 1);
        this.result = parseInt(this.result);
      }
      if (this.operator === "=") this.preValue = this.result;
      this.operator === ""
        ? (this.preValue = this.result)
        : (this.value = this.result);
      //判斷是preValue還是value
    },
  },
};
</script>

<style lang="scss" scoped>
#calculator {
  // max-width: 28rem;
  min-width: 22rem;
  // height: auto;
  margin: auto;
  border-radius: 10px;
  padding: 10px 1rem;
  background-color: rgba(75, 85, 99, $alpha: 1);
  .row {
    --bs-gutter-x: 0.5rem;
    margin: 0.5rem 0;
    text-decoration: none;
    .account {
      text-align: start;
      color: rgb(165, 165, 165);
      display: flex;
      // height: 100%;
      align-items: center;
      margin: 0;
    }
    .screen {
      background-color: rgba(31, 41, 55, $alpha: 1);
      border-radius: 10px;
      margin: 0;
      padding: 0.5rem 1rem 0 0;
      color: white;
      text-align: right;
      h1 {
        font-size: 3rem;
      }
    }
    button {
      font-size: 1.5rem;
      width: 100%;
      // margin: 2%;
      padding: 1rem;
    }
  }
}
</style>