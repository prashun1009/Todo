<template>
  <div>

  <input type="text" class="todo-input" placeholder="Enter the text" v-model="newTodo" @keyup.enter="addTodo">
    <div v-for="(todo,index) in todosFiltered" :key="todo.id" class="todo-items">
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
     <div class="extra-container">
       <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">Check All</label></div>
       <div>{{ remaining }} Items left</div>
     </div>

     <div class="extra-container">
       <div>
         <button :class="{ active: filter == 'all'}" @click="filter = 'all'">All</button>
         <button :class="{ active: filter == 'active'}" @click="filter = 'active'">Active</button>
         <button :class="{ active: filter == 'completed'}" @click="filter = 'completed'">Completed</button>
       </div>
          <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
       <div>

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
      filter: 'all',
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

  computed: {
    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    },

    anyRemaining () {
      return this.remaining !== 0
    },

    todosFiltered () {
      if (this.filter === 'all') {
        return this.todos
      } else if (this.filter === 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter === 'completed') {
        return this.todos.filter(todo => todo.completed)
      }

      return this.todos
    },

    showClearCompletedButton () {
      return this.todos.filter(todo => todo.completed).length > 0
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
    },
    checkAllTodos () {
      this.todos.forEach((todo) => todo.completed = event.target.checked)
    },
    clearCompleted () {
      this.todos = this.todos.filter(todo => !todo.completed)
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

.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 15px;
  border-top: 1px solid black;
  padding-top: 14px;
  margin-bottom: 14px;
}
button {
  background-color: white;
  font-size: 14px;
  appearance: none;
}
button:hover {
  background: lightgreen;
}
button:focus {
  outline: none;
}

.active {
  background: lightgreen;
}
</style>
