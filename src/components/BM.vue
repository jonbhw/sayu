<template>
  <div>
    <div class="content">
      <ul>
        <li>线性复杂度为 {{ this.LCOutline.slice(-1)[0] }}</li>
        <li>线性复杂度轮廓 {{ this.LCOutline }}</li>
      </ul>
    </div>
    <b-table :data="tableData" :columns="columns"></b-table>
  </div>
</template>

<script>
export default {
  name: "BM",
  props: {
    inputText: String,
  },
  data() {
    return {
      tableData: [],
      LCOutline: [],
      columns: [
        {
          field: "j",
          label: "j",
          centered: true
        },
        {
          field: "s",
          label: "s[i]",
          centered: true
        },
        {
          field: "d",
          label: "d",
          centered: true
        },
        {
          field: "m",
          label: "m",
          centered: true
        },
        {
          field: "T",
          label: "T(D)",
          centered: true
        },
        {
          field: "C",
          label: "C(D)",
          centered: true
        },
        {
          field: "L",
          label: "L",
          centered: true
        },
        {
          field: "B",
          label: "B(D)",
          centered: true
        },
      ],
    };
  },
  watch: {
    inputText() {
      this.doBM()
    }
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
      ]
      this.LCOutline = []

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
          fm_flag = false
          fm = 0
        }
        // 计算 d = s_j XOR ...
        d = this.inputText[j];
        for (i = 1; i <= L; i++) {
          d ^= C[i] & this.inputText[j - i];
        }
        // Fake_M += 1
        fm++
        // 若 d ≠ 0 (即 d = 1)
        if (d == 1) {
          // T(D) = C(D)
          for (i = 0; i < this.inputText.length; i++) {
            T[i] = C[i];
          }
          // C(D)
          for (i = 0; (i + j - m) < this.inputText.length; i++) {
            C[i + j - m] ^= B[i];
          }
          // 若 L ≤ j/2
          if (L <= (j / 2)) {
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
        this.LCOutline.push(L)
      }
    },
  },
};
// 获得形如 D^i + D^j 的字符串输出
function Dstr(a, L) {
  var result = ""
  for (let i = 0; i <= L; i++) {
    if (a[i]) {
      if (i === 0) {
        result += "1" + "+"
      } else if (i === 1) {
        result += "D" + "+"
      }
      else {
        result += "D<sup>" + i + "</sup>+"
      }
    }
  }
  result = result.slice(0, result.length - 3)
  return result
}
</script>

<style lang="stylus" scoped>

</style>