<template>
  <div id="app">
    <Calc v-bind:title="message" v-on:result-event="appAction" />
    <hr />
    <div>
      <table v-html="log"></table>
    </div>
  </div>
</template>

<script>
import Calc from "./components/Calc.vue";

export default {
  name: "app",
  components: {
    Calc,
  },
  data: function () {
    return {
      message: "CALC",
      result: [],
    };
  },
  computed: {
    // ログテーブル生成
    log: function () {
      let table = `<tr>
                    <th class="head">Expression</th>
                    <th class="head">Value</ht>
                  </tr>`;
      const result = this.result;
      console.log(result);
      for (const ret of this.result) {
        table += `<tr>
                    <td>${ret[0]}</td>
                    <th>${ret[1]}</th></tr>`;
      }
      return table;
    },
  },

  // 初期表示用の処理
  created: function () {
    const items = localStorage.getItem("log");
    const logs = JSON.parse(items);
    if (logs != null) {
      this.result = logs;
    }
  },
  methods: {
    // 計算結果をresultとlocalstorageへ格納
    appAction: function (exp, res) {
      this.result.unshift([exp, res]);
      if (this.result.length > 10) {
        this.result.pop();
      }
      let log = JSON.stringify(this.result);
      localStorage.setItem("log", log);
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin: 5px;
}
tr td {
  padding: 5px;
  border: 1px solid gray;
}
tr th {
  padding: 5px;
  border: 1px solid gray;
}
tr th.head {
  background-color: black;
  color: white;
}
</style>
