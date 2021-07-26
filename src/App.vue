<template>
<h1 class="title">Calculator App with Vue3</h1>
  <div class="container">
      <a href="https://github.com/lcijoyce622"  class="row" target="_blank">
        <img class="col-1" src="./assets/github.svg" alt="" />
        <p class="col account">lcijoyce622</p>
        </a>

    <div class="row">
      <div class="screen">
        <h1>{{ result }}</h1>
        <p v-if="operator === '='">{{ result }}</p>
        <p v-else-if="preValue === null">請輸入數字開始計算</p>
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
  data() {
    return {
      num: 0,
      result: 0,
      operator: "",
      value: null,
      preValue: null,
      isFloat: null,
    };
  },
  methods: {
    numClick(num) {
      this.value = null;
      if (this.isFloat === true) {
        //判斷是否為小數
        console.log("有小數點");
        this.result = this.result.toString();
        // 先將數字型態轉成小數
        if (this.result.includes(".")) {
          //如果已經有按過小數點
          this.result += num;
        } else {
          // 第一次按小數點
          this.result = this.result + "." + num;
        }
        this.result = parseFloat(this.result);
        //轉回數字型態
        this.preValue = this.result;
      } else {
        if (this.operator === "") {
          if (this.result === 0) {
            // 判斷是否為個位數
            this.result = num;
            this.preValue = num;
          } else {
            //十位數
            this.result = this.result * 10 + num;
            this.preValue = this.result;
          }
        } else if (this.operator === "=") {
          //重製operator(計算出答案之後沒清空直接又鍵入新值)
          this.operator = "";
          this.result = num;
          this.preValue = num;
        } else {
          //放入第二個數值
          this.result = num;
          this.value = num;
        }
      }
    },
    operatorClick(operator) {
      this.value = null;
      this.operator = operator;
      this.isFloat = false; //按完運算符重製下一個value的小數點
    },
    getResult() {
      switch (this.operator) {
        case "+":
          this.result = this.preValue + this.value;
          break;
        case "-":
          this.result = this.preValue - this.value;
          break;
        case "*":
          this.result = this.preValue * this.value;
          break;
        case "/":
          this.result = this.preValue / this.value;
          break;
      }
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
      this.isFloat = null;
    },
    deleteResult() {
      if (this.operator === "" || this.operator === "=") {
        if (this.result < 10) {
          //個位數按DEL直接歸零
          this.result = 0;
          this.preValue = this.result;
        } else {
          //一般狀況按DEL
          this.result = this.result.toString();
          if (this.result.charAt(this.result.length - 2) === ".") {
            //判斷DEL刪到小數點的狀況
            this.result = this.result.slice(0, this.result.length - 2);
            this.isFloat = false;
            this.preValue = this.result;
          } else {
            this.result = this.result.slice(0, this.result.length - 1);
          }
          this.result = parseFloat(this.result);
          this.preValue = this.result;
        }
      }
      console.log("DEL");
      console.log(this.result);
    },
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
    .title{
      color: rgb(82, 174, 228);
    }
    .container {
      max-width: 28rem;
      border-radius: 10px;
      padding:0.5rem 1rem;
      background-color: rgba(75, 85, 99, $alpha: 1);

      .row {
        --bs-gutter-x: 0.5rem;
        margin: 0.5rem 0;
         text-decoration:none;
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
          font-size: 1.875rem;
          width: 100%;
          // margin: 2%;
          padding: 1rem;
        }
      }
    }
  }
}
</style>
