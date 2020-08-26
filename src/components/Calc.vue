<template>
  <div class="hello">
    <h1>{{title}}</h1>
    <p>{{message}}</p>
    <hr />
    <div>
      <div>
        <textarea v-model="fomula" cols="40" rows="5"></textarea>
      </div>
      <button v-on:click="doAction">CALC</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Calc",
  props: {
    title: String,
  },
  data: function () {
    return {
      message: "Enter expression:",
      fomula: "0",
    };
  },
  methods: {
    doAction: function () {
      // 入力文字列を改行毎に配列に格納
      const arrs = this.fomula.trim().split("\n");

      // 配列から最終行を取り出し
      const last = arrs.pop();

      // 配列内の文字列をjavascriptコードに変換し結合
      let fn = "";
      for (const arr of arrs) {
        if (arr.trim() != "") {
          // fn += "var " + arr + ";";
          fn += `let ${arr};`;
        }
      }

      // 最終行を追加し、入力文字列をjavascriptコードとして実行し結果を格納
      fn += `return ${last};`;
      const exp = `function f(){${fn}} f()`;
      const ans = eval(exp);

      // 結果を表示
      this.message = `answer: ${ans}`;

      // 実行した文字列をログ用に変換
      let re = arrs.join(";").trim();
      if (re != "") {
        re += ";";
      }
      re += last;

      // result-eventを実行
      this.$emit("result-event", re, ans);
    },
  },
};
</script>