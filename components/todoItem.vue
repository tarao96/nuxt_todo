<template>
<div>
<label class="todo-search-done" for="todo-search-done">完了したものを非表示にする<input type="checkbox" v-model="hideDoneTodo"></label>
<table>
  <tr>
      <th>日付</th>
      <th>ToDo</th>
      <th>カテゴリ</th>
      <th>チェック</th>
      <th>編集</th>
      <th>削除</th>
  </tr>
  <tr v-for="todo in filterTodos" :key="todo">
      <td>{{ todo.dateTime }}</td>
      <td>{{ todo.body }}</td>
      <td>{{ todo.category }}</td>
      <td><input class="checkbox" type="checkbox" :checked="todo.done" v-model="todo.done"></td>
      <td><button>編集</button></td>
      <td><button @click="deleteTodo(todo)">削除</button></td>
  </tr>
</table>
</div>

</template>

<script>
import axios from 'axios';

export default ({
    props: {
      hideDoneTodo: {
        type: Boolean,
        required: true,
      },
      todos: {
        type: Array
      },
      searchWord: {
        type: String
      }
    },
    computed: {
      filterTodos: function() {
        const searchWord = this.searchWord;
        const hideDoneTodo = this.hideDoneTodo;
        return this.todos
          .filter(function(todo) {
            return todo.body.indexOf(searchWord) !== -1;
          })
          .filter(function(todo) {
            console.log('hidden');
            if(hideDoneTodo) {
              return !todo.done;
            } else {
              return true;
            }
          })
      },
    },
    methods: {
      fetchItem: async function() {
        const url = "http://localhost:3000/json/item.json";
        const response = await axios.get(url);
        console.log(response.data);
        this.todos = response.data;
        console.log(this.todos);
      },
    },
    mounted: function() {
      this.fetchItem();
    }
})
</script>

<style scoped>

</style>

