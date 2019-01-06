<template>
  <div>

  <input type="text" class="todo-input" placeholder="Enter the text" v-model="newTodo" @keyup.enter="addTodo">
    <div v-for="(todo,index) in todos" :key="todo.id" class="todo-items">
      <div class="todo-item-left">
        <input type="checkbox" v-model="todo.completed" class="completed">
        <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label">{{ todo.title }}</div>
        <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)"
        @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
      </div>
      <div class="remove-item" @click="removeTodo(index)">
        &times;
      </div>
     </div>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
      beforeEditCache: '',
      todos: [
        {
          'id': 1,
          'title': 'finish vue',
          'completed': false,
          'editing': false
        },
        {
          'id': 2,
          'title': 'take over world',
          'completed': false,
          'editing': false
        }
      ]
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  methods: {
    // if(this.newTodo.trim.().length = 0) {
    //   return
    // }
    addTodo () {
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false
      })
      this.newTodo = ''
      this.idForTodo++
    },
    editTodo (todo) {
      this.beforeEditCache = todo.title
      todo.editing = true
    },
    doneEdit (todo) {
      todo.editing = false
    },
    cancelEdit (todo) {
      todo.title = this.beforeEditCache
      todo.editing = false
    },
    removeTodo (index) {
      this.todos.splice(index, 1)
    }
  }
}
</script>

<style scoped>
.todo-input {
  width: 100%;
  font-size: 18px;
  padding: 10px 18px;
  margin-bottom: 16px;
}

.remove-item {
  cursor: pointer;
  margin-top: -28px;
  float: right;
}

.todo-item-left {
  padding-left: 12px;
}

.todo-item-label {
  padding: 10px;
  border: 1px solid white;
}

.todo-item-edit {
  font-size: 24px;
  color: black;
  margin-top: 4px;
  padding: 10px;
  border: 1px solid #ccc;
}
.completed {
  margin-top: 16px;
  float: left;

}
</style>
