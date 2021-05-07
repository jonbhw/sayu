<template>
  <div>
    <div class="content">
      <ul>
        <li>线性复杂度为 {{ linearComplexity }}</li>
        <li>线性复杂度轮廓 {{ this.LCOutline }}</li>
      </ul>
    </div>
    <b-field>
      <b-checkbox v-model="enableTable"
        >显示结果表格（可能导致严重卡顿）</b-checkbox
      >
    </b-field>
    <b-collapse
      aria-id="panel"
      class="panel"
      animation="slide"
      v-model="isOpen"
      v-if="enableTable"
    >
      <template #trigger>
        <div class="panel-heading" role="button" aria-controls="panel">
          结果表格
        </div>
      </template>
      <b-table
        :height="500"
        :data="tableData"
        :mobile-cards="false"
        :sticky-header="true"
        :striped="true"
      >
        <b-table-column field="j" label="j" v-slot="props">
          {{ props.row.j }}
        </b-table-column>
        <b-table-column field="s" label="s[j]" v-slot="props">
          {{ props.row.s }}
        </b-table-column>
        <b-table-column field="d" label="d" v-slot="props">
          {{ props.row.d }}
        </b-table-column>
        <b-table-column field="m" label="m" v-slot="props">
          {{ props.row.m }}
        </b-table-column>
        <b-table-column field="L" label="L" v-slot="props">
          {{ props.row.L }}
        </b-table-column>
        <b-table-column field="T" label="T(D)" v-slot="props">
          <span v-html="props.row.T"></span>
        </b-table-column>
        <b-table-column field="C" label="C(D)" v-slot="props">
          <span v-html="props.row.C"></span>
        </b-table-column>
        <b-table-column field="B" label="B(D)" v-slot="props">
          <span v-html="props.row.B"></span>
        </b-table-column>
      </b-table>
    </b-collapse>
  </div>
</template>

<script>
const MAX_LINEAR_COMPLEXITY = 10000

export default {
  name: "BM",
  props: {
    inputText: String,
  },
  data() {
    return {
      tableData: [],
      LCOutline: [],
      isOpen: false,
      enableTable: false,
    };
  },
  computed: {
    linearComplexity() {
      return (this.LCOutline.slice(-1)[0] <= MAX_LINEAR_COMPLEXITY) ? this.LCOutline.slice(-1)[0] : `超过 ${MAX_LINEAR_COMPLEXITY}（最大线性复杂度限制）`
    }
  },
  watch: {
    inputText() {
      this.doBM();
    },
  },
  mounted() {
    this.doBM()
  },
  methods: {
    doBM() {
      this.tableData = [
        {
          j: "",
          s: "",
          d: "",
          m: 0,
          T: "",
          C: 1,
          L: 0,
          B: 1,
        },
      ];
      this.LCOutline = [];

      var B = [];
      var T = [];
      var C = [];
      for (let i = 0; i < this.inputText.length; i++) {
        B.push(0);
        T.push(0);
        C.push(0);
      }
      B[0] = 1;
      C[0] = 1;
      var L = 0;
      var m = -1;
      var fm = 0; // Fake_M
      var fm_flag = false; // Fake_M 在下一轮应该归零
      var d, i;

      for (let j = 0; j < this.inputText.length; j++) {
        // 执行上次循环所指示的 "Fake_M 需要归零"
        if (fm_flag) {
          fm_flag = false;
          fm = 0;
        }
        // 计算 d = s_j XOR ...
        d = this.inputText[j];
        for (i = 1; i <= L; i++) {
          d ^= C[i] & this.inputText[j - i];
        }
        // Fake_M += 1
        fm++;
        // 若 d ≠ 0 (即 d = 1)
        if (d == 1) {
          // T(D) = C(D)
          for (i = 0; i < this.inputText.length; i++) {
            T[i] = C[i];
          }
          // C(D)
          for (i = 0; i + j - m < this.inputText.length; i++) {
            C[i + j - m] ^= B[i];
          }
          // 若 L ≤ j/2
          if (L <= j / 2) {
            L = j + 1 - L;
            m = j;

            // Fake_M = 0, 此处不直接设为零是因为表格输出需要
            fm_flag = true;

            for (i = 0; i < this.inputText.length; i++) {
              B[i] = T[i];
            }
          }
        }
        this.tableData.push({
          j: j,
          s: this.inputText[j],
          d: d,
          m: fm,
          T: Dstr(T, L),
          C: Dstr(C, L),
          L: L,
          B: Dstr(B, L),
        });
        // 线性复杂度轮廓
        this.LCOutline.push(L);
        if (L > MAX_LINEAR_COMPLEXITY) {
          this.tableData.push({
            j: "...",
            s: "...",
            d: "...",
            m: "...",
            L: "...",
            T: "...",
            C: "...",
            B: "...",
          });
          break;
        }
      }
    },
  },
};
// 获得形如 D^i + D^j 的字符串输出
function Dstr(a, L) {
  var result = "";
  for (let i = 0; i <= L; i++) {
    if (a[i]) {
      if (i === 0) {
        result += "1" + "+";
      } else if (i === 1) {
        result += "D" + "+";
      } else {
        result += "D<sup>" + i + "</sup>+";
      }
    }
  }
  result = result.slice(0, result.length - 1);
  return result;
}
</script>

<style lang="stylus" scoped></style>