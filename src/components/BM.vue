<template>
  <div>
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
      columns: [
        {
          field: "j",
          label: "j",
        },
        {
          field: "s",
          label: "s[i]",
        },
        {
          field: "d",
          label: "d",
        },
        {
          field: "m",
          label: "m",
        },
        {
          field: "T",
          label: "T(D)",
        },
        {
          field: "C",
          label: "C(D)",
        },
        {
          field: "L",
          label: "L",
        },
        {
          field: "B",
          label: "B(D)",
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
      var d, i;

      for (let j = 0; j < this.inputText.length; j++) {
        d = Number(this.inputText[j]);
        for (i = 1; i < L; i++) {
          d ^= C[i] & Number(this.inputText[j - i]);
        }
        if (d == 1) {
          for (i = 0; i < this.inputText.length; i++) {
            T[i] = C[i];
          }
          for (i = 0; (i + j - m) < this.inputText.length; i++) {
            C[i + j - m] ^= B[i];
          }
          if (L <= (j >> 1)) {
            L = j + 1 - L;
            m = j;

            for (i = 0; i < this.inputText.length; i++) {
              B[i] = T[i];
            }
          }
        }
        this.tableData.push({
          j: j,
          s: this.inputText[j],
          d: d,
          m: m,
          T: Dstr(T, L),
          C: Dstr(C, L),
          L: L,
          B: Dstr(B, L),
        });
      }
    },
  },
};
function Dstr(a, L) {
  var result = ""
  for (let i = 0; i <= L; i++) {
    if (a[i]) {
      if (i === 0) {
        result += "1" + " + "
      }
      else {
        result += "D^" + i + " + "
      }
    }
  }
  result = result.slice(0, result.length - 3)
  return result
}
</script>

<style lang="stylus" scoped>

</style>