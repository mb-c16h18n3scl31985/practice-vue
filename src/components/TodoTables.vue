<template>
    <div>
        <p>{{ state }}</p>

        <table>
            <thead>
            <tr>
                <th scope="col" colspan="4">{{ tableTitle(state) }}</th>
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
                    <button @click="doChangeState(item)">{{ changeStateWord(state) }}</button>
                </td>
                <td class="button">
                    <button @click.ctrl="doRemove(item)">削除</button>
                </td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    data() {
        return {
            options: {
                all: {state: -1, japanese: 'すべて'},
                working: {state: 0, japanese: '作業中', changeState: 'completed'},
                completed: {state: 1, japanese: '完了', changeState: 'working'}
            },
        }
    },

    props: {
        todos: Array,
        state: String
    },

    methods: {
        // タイトルを返す
        tableTitle: function (state) {
            if (state === 'working') {
                return this.options.working.japanese
            } else if (state === 'completed') {
                return this.options.completed.japanese
            }
        },

        // ボタン変更文句
        changeStateWord: function (state){
            if (state === 'working') {
                return '完了へ移動'
            } else if (state === 'completed') {
                return '実はまだ終わってない'
            }
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
</script>

<style scoped>
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