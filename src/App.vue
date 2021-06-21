<template>
  <div class='app__main'>
    <!-- 絞り込みラジオボタン -->
    <!-- テーブル -->
    <!-- 新規登録フォーム -->
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data() {
    return {
      // 使用するデータ
    }
  },
  methods: {
    // 使用するメソッド
  }
}


</script>

<style>
.app__main {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  /* 以上CSSは元々あったもの */
  /* 以下参考main.cssよりコピペ */
  /* @see: https://github.com/mio3io/cr-vue/blob/master/codes/tutorial-todo/main.css */

  max-width: 640px;
  margin: 0 auto;
}

* {
  box-sizing: border-box;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead th {
  border-bottom: 2px solid #0099e4; /*#d31c4a */
  color: #0099e4;
}

th,
td {
  padding: 0 8px;
  line-height: 40px;
}

thead th.id {
  width: 50px;
}

thead th.state {
  width: 100px;
}

thead th.button {
  width: 60px;
}

tbody td.button, tbody td.state {
  text-align: center;
}

tbody tr td,
tbody tr th {
  border-bottom: 1px solid #ccc;
  transition: all 0.4s;
}

tbody tr.done td,
tbody tr.done th {
  background: #f8f8f8;
  color: #bbb;
}

tbody tr:hover td,
tbody tr:hover th {
  background: #f4fbff;
}

button {
  border: none;
  border-radius: 20px;
  line-height: 24px;
  padding: 0 8px;
  background: #0099e4;
  color: #fff;
  cursor: pointer;
}
</style>
