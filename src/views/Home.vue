<template>
  <div class="home">
    <section class="section">
      <div class="container">
        <!-- 比特串输入 -->
        <b-field
          label="比特串输入"
        >
          <b-input
            v-model="inputText"
            type="textarea"
            placeholder="待解比特串"
          ></b-input>
        </b-field>
        <div class="buttons is-right">
          <b-button type="is-light" @click="randomData">随机数据</b-button>
          <b-button @click="doProcess">执行</b-button>
        </div>
        <!-- 结果 -->
        <section class="section" v-show="BMText">
          <h2 class="title is-4">结果</h2>
          <BM :inputText="BMText" />
        </section>
      </div>
    </section>
  </div>
</template>

<script>
import BM from "@/components/BM.vue";

export default {
  name: 'Home',
  components: {
    BM
  },
  data() {
    return {
      inputText: "",
      BMText: ""
    }
  },
  watch: {
    inputText() {
      this.BMText = ""
    }
  },
  methods: {
    randomData() {
      const len = getRandomIntInclusive(1, 10)
      var array = new Uint32Array(len);
      window.crypto.getRandomValues(array);
      var result = ""
      for (let i of array) {
        result += decimalToBinary(i)
      }
      this.inputText = result
    },
    doProcess() {
      this.BMText = this.inputText.replace(/\s+/g, "")
    }
  }
}

// [min, max] 中随机整数
function getRandomIntInclusive(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min + 1)) + min; //含最大值，含最小值
}

// https://juejin.cn/post/6844904201953214478
/*
  * EQU - 等于：equal 
  * NEQ - 不等于：not equal
  * LSS - 小于：less than
  * LEQ - 小于或等于：equal or less than
  * GTR - 大于：greater than
  * GEQ - 大于或等于：equal or greater than
*/

/**
* @name stringAddtion
* @description 字符串求和
* @param {string} a 加数一
* @param {string} b 加数二
* @param {any} base 基数（默认十进制）
* @return 返回计算结果
*/
const stringAddtion = (a = '', b = '', base = 10) => {
  // 结果
  let result = '';
  // 进位标记
  let carry = 0;
  // 设置 a、b 的长度，方便逆序遍历
  let aIndex = a.length - 1, bIndex = b.length - 1;
  while (aIndex >= 0 || bIndex >= 0) { // a 或 b 还有位可以相加
    // aIndex bIndex可能为负数值，需要转化为 0
    let sum = (+a[aIndex] || 0) + (+b[bIndex] || 0) + carry;
    // 是否需要进位
    carry = sum >= base ? 1 : 0;
    // 计算最终结果
    result = sum % base + result;
    // 移位后往更高位靠
    aIndex--;
    bIndex--;
  }
  // 如果计算完毕后还有进位，那么前面 + 1
  if (carry) {
    result = '1' + result;
  }
  // 返回最终结果
  return result;
};

/**
* @name integerLEQZero
* @description 大于或者等于 0 的整数转换为二进制
* @param {number} number
* @return 二进制字符串
*/
const integerGEQZero = (number) => {
  let result = '';
  while (number >= 1) {
    result = number % 2 + result;
    number = Math.floor(number / 2);
  }
  return result;
};

/**
* @name integerLSSZero
* @description 小于 0 的整数转换为二进制
* @param {number} number
* @return 二进制字符串
*/
const integerLSSZero = (number) => {
  let result = '';
  // 转正
  number = -number;
  // 求二进制并取反
  while (number >= 1) {
    if (number % 2 === 1) {
      result = '0' + result;
    } else {
      result = '1' + result;
    }
    number = Math.floor(number / 2);
  }
  // 补全
  if (result.length < 8) {
    result = '1'.repeat(8 - result.length) + result;
  }
  // 加一
  result = stringAddtion(result, '1', 2);
  return result;
};

/**
* @name floorGTROne
* @description 大于 1 的小数转二进制
* @param {number} number
* @return 二进制字符串
*/
const floorGTROne = (number) => {
  let result = '';
  const integer = Math.floor(number);
  const decimal = number - integer;
  result += integerGEQZero(integer) + '.' + floorBetweenZeroOne(decimal).split('.')[1];
  return result;
};

/**
* @name floorBetweenZeroOne
* @description 大于 0 小于 1 的小数转二进制
* @param {number} number
* @return 二进制字符串
*/
const floorBetweenZeroOne = (number) => {
  let result = '';
  while (
    number != 0
    && number != 1 // 取值到 0 或者 1 为止
    && result.length < 32 // 或者长度达到需要
  ) {
    // 取小数部分
    if (number > 1) {
      number = number - 1;
    }
    result = result + Math.floor(number * 2);
    number = number * 2;
  }
  result = '0.' + result;
  return result;
};

/**
* @name decimalToBinary
* @description 正负整数转二进制
* @param {any} number 需要转换的数字
* @return 二进制字符串
*/
const decimalToBinary = (number) => {
  let result = '';
  
  if (Number.isInteger(number) && number >= 0) { // 正整数
    result = integerGEQZero(number);
  } else if (Number.isInteger(number) && number < 0) { // 负整数
    result = integerLSSZero(number);
  } else if (!Number.isInteger(number) && number > 0 && number < 1) { // 正小数（不带整数位）
    result = floorBetweenZeroOne(number);
  } else if (!Number.isInteger(number) && number > 1) { // 正小数（带整数位）
    result = floorGTROne(number);
  }

  return result;
};
</script>
