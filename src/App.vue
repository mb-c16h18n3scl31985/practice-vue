<template>
    <div class='app__main'>
        <h1>ToDoリスト</h1>

        <!-- 絞り込みラジオボタン -->
<!--        <label v-for="(label,key) in options" :key='key'>-->
<!--            <input type="radio" v-model="current" v-bind:value="label.value">{{ label.label }}-->
<!--        </label>-->

        <!-- タスク表示テーブル -->
        <div class="tables">
            <TodoTables :todos="todos"></TodoTables>
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
import TodoTables from "./components/TodoTables";

export default {
    name: 'App',
    components:{
        TodoTables
    },
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

.tables {
    width: 100%;
    display: flex;
    justify-content: space-around;
}

.tables table {
    flex-basis: 47%;
}

</style>
