<template>
  <div class='app__main'>
    <!-- 絞り込みラジオボタン -->

    <table>
      <thead>
      <tr>
        <th class="id" scope="col">ID</th>
        <th class="comment" scope="col">コメント</th>
        <th class="state" scope="col">状態</th>
        <th class="button" scope="col">-</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="item in todos" :key="item.id">
        <th scope="row">{{ item.id }}</th>
        <td>{{ item.comment }}</td>
        <td class="state">
          <!-- 状態変更ボタンのモック -->
          <button>{{ item.state }}</button>
        </td>
        <td class="button">
          <!-- 削除ボタンのモック -->
          <button>削除</button>
        </td>
      </tr>
      </tbody>
    </table>

    <h2>新しい作業の追加</h2>
    <form class="add-form" @submit.prevent="doAdd">
      <label for="comment">コメント</label>
      <input id="comment" type="text" ref="comment">
      <button type="submit">追加</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {},
  data() {
    return {
      // 使用するデータ
      todos: [{id:'1',comment:'にんじん',state:'waiting'}]
    }
  },
  methods: {
    // 使用するメソッド
  }
}

// @see: https://jp.vuejs.org/v2/examples/todomvc.html
let STORAGE_KEY = 'todos-vuejs-demo'
let todoStorage = {
  fetch: function () {
    let todos = JSON.parse(
        localStorage.getItem(STORAGE_KEY) || '[]'
    )
    todos.forEach(function (todo, index) {
      todo.id = index
    })
    todoStorage.uid = todos.length
    return todos
  },
  save: function (todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
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
