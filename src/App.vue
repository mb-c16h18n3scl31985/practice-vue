<template>
    <div class='app__main'>
        <h1>ToDoリスト</h1>

        <!-- タスク表示テーブル -->
        <div class="tables">
            <TodoTables :todos="computedWorkingTodos" :state="state.working"></TodoTables>
            <TodoTables :todos="computedCompletedTodos" :state="state.completed"></TodoTables>
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
    components: {
        TodoTables
    },
    data() {
        return {
            // 空のオブジェクトを用意
            todos: [],

            // コメント用の枠
            comment: '',
            state: {
                all: 'all',
                working: 'working',
                completed: 'completed'}
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

.tables {
    width: 100%;
    display: flex;
    justify-content: space-around;
}

.tables * {
    flex-basis: 47%;
}

</style>
