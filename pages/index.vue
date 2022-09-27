<template>
  <div>
    <header>
      <nav>
        <h1 @click="isVisible = false">Demo Page</h1>
        <ul class="nav-list">
          <li class="nav-item" @click="displayCreate()">
            新規追加
          </li>
        </ul>
      </nav>
    </header>

    <main>
      <div class="container">
        <div class="todo-wrapper" v-show="!isVisible">
          <div class="search-box">
            <label for="search">検索</label>
            <input
              type="text"
              class="search"
              placeholder="検索したいToDoを入力してください"
              v-model="searchWord" />
            <br>
          </div>
          <todo-item
            :hideDoneTodo="hideDoneTodo"
            :todos="todos"
            :searchWord="searchWord">
          </todo-item>
        </div>
        </div>
        <div class="create-wrapper" v-show="isVisible">
          <h1>新規追加</h1>
          <form class="create-form" @submit.prevent="createTodo()">
            <label for="todo">ToDo</label>
            <input
              type="text"
              class="create"
              placeholder="新しいToDoを入力してください"
              v-model="todo">
            <select class="select" v-model="category">
              <option
                v-for="category in categories"
                :key="category">
                {{ category }}
              </option>
            </select>
            <button type="submit">送信</button>
          </form>
        </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data: function() {
    return {
      todos: [],
      todo: "",
      isVisible: false,
      categories: [
        "勉強",
        "仕事",
        "家事",
        "遊び"
      ],
      category: "",
      searchWord: "",
      hideDoneTodo: false,
    }
  },
  watch: {
    todos: {
      handler: function(next) {
        console.log('watchが反応しました')
        if(process.browser) {
          window.localStorage.setItem('todos', JSON.stringify(next));
        }
      },
      deep: true
    }
  },
  methods: {
    createTodo: function() {
      console.log('新規追加処理が走りました')
      const today = new Date();
      const date = today.getFullYear() + "-" + (today.getMonth() + 1) + "-" + today.getDate();

      if(this.todo && this.category) {
        this.todos.push({
          dateTime: date,
          body: this.todo,
          category: this.category,
          done: false
        });
        this.isVisible = false;
      }
    },
    displayCreate: function() {
      console.log('新規追加が押されました')
      this.isVisible = true;
    },
    deleteTodo: function(todo) {
      const index = this.todos.indexOf(todo);
      if(index !== -1) {
        this.todos.splice(index);
      }
    },
    todoCheck: function() {
      this.todos.check
    }
  },
  created: function() {
    if(process.browser) {
      const todos = window.localStorage.getItem('todos');

      if(todos) {
        this.todos = JSON.parse(todos);
      }
    }
  },
}
</script>

<style lang="scss">
.container {
  width: 80vw;
  text-align: center;
}

.nav-list {
  .nav-item {
    cursor: pointer;
  }
}
.main-image {
  text-align: center;
  img {
    width: 1000px;
    height: 1000px;
    object-fit: contain;
  }
}

nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 30px;
  background-color: rgb(147, 147, 232);
  h1 {
    cursor: pointer;
  }
}

.nav-list {
  display: flex;
  gap: 20px;
}

.search-box {
  margin: 100px auto;
  text-align: center;
  .search {
    height: 30px;
    width: 400px;
  }
}

table {
  margin: 0 auto;
  border-collapse: collapse;
}

th,td {
  border: 1px solid black;
  padding: 10px;
}

.create-wrapper {
  width: 60vw;
  margin: 0 auto;
  .create-form {
    text-align: center;
    .create {
      width: 250px;
      height: 30px;
    }
  }
}

.select {
  height: 30px;
  width: 50px;
}

.checkbox {
  height: 20px;
  width: 20px;
  text-align: center;
}

.todo-search-done {
  margin-bottom: 30px;
}

</style>

