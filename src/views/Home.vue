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
            @input="BMText = ''"
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
      this.BMText = this.inputText.replace(/[^01]+/g, "")
      this.inputText = this.inputText.replace(/[^01\s]+/g, "")
    }
  }
}

// [min, max] 中随机整数
function getRandomIntInclusive(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min + 1)) + min; //含最大值，含最小值
}

function decimalToBinary(num) {
  var str = parseInt(num).toString(2);
  return str
}

</script>
