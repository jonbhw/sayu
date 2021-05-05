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
          <b-button type="is-light" @click="loadExample">载入示例</b-button>
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
    loadExample() {

    },
    randomData() {

    },
    doProcess() {
      this.BMText = this.inputText
    }
  }
}
</script>
