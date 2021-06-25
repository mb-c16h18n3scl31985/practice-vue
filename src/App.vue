<template>
    <div class='app__main'>
        <h1>ToDoリスト</h1>

        <!-- 絞り込みラジオボタン -->
        <label v-for="(label,key) in options" :key='key'>
            <input type="radio" v-model="current" v-bind:value="label.value">{{ label.label }}
        </label>

        <!-- タスク表示テーブル -->
        <div class="tables">
            <table>
                <thead>
                <tr>
                    <th scope="col" colspan="4">作業中</th>
                </tr>
                <tr>
                    <th class="id" scope="col">ID</th>
                    <th class="comment" scope="col">コメント</th>
                    <th class="state" scope="col">移動</th>
                    <th class="button" scope="col">-</th>
                </tr>
                </thead>
                <tbody>
                <tr v-for="item in computedWorkingTodos" :key="item.id">
                    <th scope="row">{{ item.id }}</th>
                    <td>{{ item.comment }}</td>
                    <td class="state">
                        <button @click="doChangeState(item)">完了した</button>
                    </td>
                    <td class="button">
                        <button @click.ctrl="doRemove(item)">削除</button>
                    </td>
                </tr>
                </tbody>
            </table>

            <table>
                <thead>
                <tr>
                    <th scope="col" colspan="4">完了</th>
                </tr>
                <tr>
                    <th class="id" scope="col">ID</th>
                    <th class="comment" scope="col">コメント</th>
                    <th class="state" scope="col">移動</th>
                    <th class="button" scope="col">-</th>
                </tr>
                </thead>
                <tbody>
                <tr v-for="item in computedCompletedTodos" :key="item.id">
                    <th scope="row">{{ item.id }}</th>
                    <td>{{ item.comment }}</td>
                    <td class="state">
                        <button @click="doChangeState(item)">まだ終わってない</button>
                    </td>
                    <td class="button">
                        <button @click.ctrl="doRemove(item)">削除</button>
                    </td>
                </tr>
                </tbody>
            </table>
        </div>

        <h2>新しい作業の追加</h2>
        <form class="add-form" @submit.prevent="doAdd">
            <label for="comment">コメント</label>
            <input id="comment" type="text" v-model="comment">
            <button type="submit">追加</button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'App',
    data() {
        return {
            // 空のオブジェクトを用意
            todos: [],

            // コメント用の枠
            comment: '',

            // 選択している options の value を記憶するためのデータ
            options: [
                {value: -1, label: 'すべて'},
                {value: 0, label: '作業中'},
                {value: 1, label: '完了'}
            ],
        }
    },

    methods: {
        // タスク追加
        doAdd: function () {
            // 入力がなければ何もしないで return
            if (!this.comment) {
                return
            }
            // { 新しいID, コメント, 作業状態 }というオブジェクトを現在の todos リストへ push
            // 作業状態「state」はデフォルト「作業中=0」で作成
            this.todos.push({
                id: todoStorage.uid++,
                comment: this.comment,
                state: 0
            })
            // フォーム要素を空にする
            this.comment = ''
        },

        // 状態変更
        doChangeState: function (item) {
            item.state = item.state ? 0 : 1
        },
        // タスク削除
        doRemove: function (item) {
            let index = this.todos.indexOf(item)
            this.todos.splice(index, 1)
        },
    },

    // ウォッチャ
    // @see: https://jp.vuejs.org/v2/guide/computed.html
    // データの変更に応じ、非同期やコストの高い処理を実行したいとき有用
    // オプションを使う場合はオブジェクト形式にする
    watch: {
        todos: {
            // 引数はウォッチしているプロパティの変更後の値
            handler: function (todos) {
                todoStorage.save(todos)
            },
            // deep オプションでネストしているデータも監視できる
            deep: true
        }
    },

    // Vueインスタンス生成時の処理
    created() {
        // インスタンス作成時に自動的に fetch() する
        this.todos = todoStorage.fetch()
    },

    // 算出プロパティ
    // データから別の新しいデータを作成する関数型のデータ
    // @see: https://jp.vuejs.org/v2/guide/computed.html
    computed: {
        computedWorkingTodos: function () {
            // 作業中のタスクを返却
            return this.todos.filter(function (el) {
                return 0 === el.state
            }, this)
        },
        computedCompletedTodos: function () {
            // 完了したタスクを返却
            return this.todos.filter(function (el) {
                return 1 === el.state
            }, this)
        }
    },
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

.tables {
    width: 100%;
    display: flex;
    justify-content: space-around;
}

.tables table {
    flex-basis: 47%;
}
</style>
