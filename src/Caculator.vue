<template>
    <div id="app">
            <div class="container-displayContext">
                <el-row>
                    <el-col :span="24">
                        <div class="grid-content bg-purple-dark displayContext">
                            {{displayContext || 0}}
                        </div>
                    </el-col>
                </el-row>
            </div>
            <div class="caculator-context">
                <el-row :gutter="5">
                    <el-col :span="6">
                        <el-button plain @click="handleInput('add')">+</el-button>
                    </el-col>
                    <el-col :span="6">
                        <el-button plain @click="handleInput('substract')">-</el-button>
                    </el-col>
                    <el-col :span="6">
                        <el-button plain @click="handleInput('multiple')">x</el-button>
                    </el-col>
                     <el-col :span="6">
                        <el-button plain @click="handleInput('divide')">/</el-button>
                    </el-col>
                </el-row>
                <el-row :gutter="5">
                    <el-col :span="6">
                        <el-button plain @click="handleInput('7')">7</el-button>
                    </el-col>
                    <el-col :span="6">
                          <el-button plain @click="handleInput('8')">8</el-button>
                    </el-col>
                    <el-col :span="6">
                         <el-button plain @click="handleInput('9')">9</el-button>
                    </el-col>
                    <el-col :span="6">
                            <el-button plain @click="handleInput('AC')">AC</el-button>
                    </el-col>
                </el-row>
                <el-row :gutter="5">
                    <el-col :span="6">
                        <el-button plain @click="handleInput('4')">4</el-button>
                    </el-col>
                    <el-col :span="6">
                          <el-button plain @click="handleInput('5')">5</el-button>
                    </el-col>
                    <el-col :span="6">
                         <el-button plain @click="handleInput('6')">6</el-button>
                    </el-col>
                    <el-col :span="6">
                            <el-button plain @click="handleInput('need')">need</el-button>
                    </el-col>
                </el-row>
                <el-row :gutter="5">
                    <el-col :span="6">
                        <el-button plain @click="handleInput('1')">1</el-button>
                    </el-col>
                    <el-col :span="6">
                          <el-button plain @click="handleInput('2')">2</el-button>
                    </el-col>
                    <el-col :span="6">
                         <el-button plain @click="handleInput('3')">3</el-button>
                    </el-col>
                    <el-col :span="6">
                            <el-button plain @click="handleInput('need')">need</el-button>
                    </el-col>
                </el-row>
                <el-row :gutter="5">
                    <el-col :span="6">
                        <el-button plain @click="handleInput('0')">0</el-button>
                    </el-col>
                    <el-col :span="6">
                          <el-button plain @click="handleInput('.')">.</el-button>
                    </el-col>
                    <el-col :span="12">
                            <el-button class="btn-red-light" plain @click="handleInput('=')">=</el-button>
                    </el-col>
                </el-row>
            </div>
    </div>

</template>

<script>
export default {
  data() {
    return {
      firstNumber: undefined,
      secondNumber: undefined,
      operator: undefined,
      result: undefined,
      inputValue: undefined,
      displayContext: 0,
    };
  },
  methods: {
    caculate(firstNumber, secondNumber, operator) {
      let result;

      if (firstNumber && secondNumber && operator) {
        switch (operator) {
          case "add":
            result = firstNumber + secondNumber;
            break;
          case "substract":
            result = firstNumber - secondNumber;
            break;
          case "multiple":
            result = firstNumber * secondNumber;
            break;
          case "divide":
            result = firstNumber / secondNumber;
            break;
          default:
            break;
        }
      }

      return result;
    },
    parseInputToNumber(input) {
      let output;

      if (input.includes(".")) {
        output = parseFloat(input);
      } else {
        output = parseInt(input);
      }

      return output;
    },
    handleInput(character) {
      const isOperator = inputString =>
        ["add", "substract", "multiple", "divide"].includes(inputString);

      switch (character) {
        case "AC":
          this.firstNumber = undefined;
          this.secondNumber = undefined;
          this.operator = undefined;
          this.result = undefined;
          this.inputValue = undefined;
          this.displayContext = undefined;
          break;

        case "=":
          if (this.firstNumber && this.secondNumber && this.operator) {
            this.result = this.caculate(
              this.firstNumber,
              this.secondNumber,
              this.operator
            );
            this.setDisplayContext(this.result);
            console.log("result", this.result);
          }
          break;

        default:
          if (this.result) {
            if (isOperator(character)) {
                this.firstNumber = this.result;
                this.secondNumber = undefined;
                this.operator = undefined;
                this.result = undefined;
                this.inputValue = undefined;
                this.displayContext = undefined;
            } else {
                this.firstNumber = undefined;
                this.secondNumber = undefined;
                this.operator = undefined;
                this.result = undefined;
                this.inputValue = undefined;
                this.displayContext = undefined;
            }
          }
          if (!this.operator && !this.secondNumber) {
            if (!isOperator(character)) {
              this.inputValue = this.inputValue
                ? this.inputValue.concat(character)
                : character;
              this.firstNumber = this.parseInputToNumber(this.inputValue);
              this.setDisplayContext(this.firstNumber);
              console.log("first number", this.firstNumber);
            } else {
              this.operator = character;
              this.inputValue = undefined;
              console.log("operator", this.operator);
            }
          } else {
            console.log(character);
            this.inputValue = this.inputValue
              ? this.inputValue.concat(character)
              : character;
            this.secondNumber = this.parseInputToNumber(this.inputValue);
            console.log("second number", this.secondNumber);
             this.setDisplayContext(this.secondNumber);
          }
      }
    },
    setDisplayContext(displayContext) {
        this.displayContext = displayContext
    }
  }
};
</script>

<style>
  .el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .btn-red-light {
    background-color:#e04204;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
</style>