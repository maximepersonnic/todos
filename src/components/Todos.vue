<template>
  <section class="todoapp">
    <header class="header">
      <h1>todos</h1>
      <input type="text" class="new-todo" placeholder="Add a task" v-model="newTodo" @keyup.enter="addTodo">
    </header>
    <div class="main">
      <input id="toggle-all" type="checkbox" class="toggle-all" v-model="allDone">
      <label for="toggle-all">"Marquer comme complétée</label>
      <ul class="todo-list">
        <li class="todo" v-for="todo in filteredTodos" :class="{completed: todo.completed, editing: todo === editing}">
          <div class="view">
            <input type="checkbox" v-model="todo.completed" class="toggle">
             <label @dblclick="editTodo(todo)">{{ todo.name }}</label>
             <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
          <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" v-focus="todo === editing" @blur="doneEdit" @keyup.esc="cancelEdit">
        </li>
      </ul>
    </div>
    <footer class="footer" v-show="hasTodos">
      <span class="todo-count"><strong>{{ remaining }}</strong> to do tasks</span>
      <ul class="filters">
        <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">All</a></li>
        <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">To do</a></li>
        <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Done</a></li>
      </ul>
      <button class="clear-completed" v-show="completed" @click.prevent="deleteCompleted">Delete done tasks</button>
    </footer>
  </section>
</template>

<script>
import Vue from 'vue'

export default {
  data () {
    return {
      todos: [{
        name: 'Test task',
        completed: false
      }],
      newTodo: '',
      filter: 'all',
      editing: null,
      oldTodo: ''
    }
  },
  methods: {
    addTodo () {
      this.todos.push({
        completed: false,
        name: this.newTodo
      })
      this.newTodo = ''
    },
    deleteTodo (todo) {
      this.todos = this.todos.filter(i => i !== todo)
    },
    deleteCompleted () {
      this.todos = this.todos.filter(todo => !todo.completed)
    },
    editTodo (todo) {
      this.editing = todo
      this.oldTodo = todo.name
    },
    doneEdit () {
      this.editing = null
    },
    cancelEdit () {
      this.editing.name = this.oldTodo
      this.doneEdit()
    }
  },
  computed: {
    allDone: {
      get () {
        return this.remaining === 0

      },

      set (value) {
        this.todos.forEach(todo => {
          todo.completed = value
        })
      }
    },
    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    },
    completed () {
      return this.todos.filter(todo => todo.completed).length > 0
    },
    hasTodos () {
      return this.todos.length > 0
    },
    filteredTodos () {
      if(this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed)
      } else if(this.filter === 'done') {
        return this.todos.filter(todo => todo.completed)
      } else {
        return this.todos
      }
    }
  },
  directives: {
    focus (el, value) {
      if (value) {
        Vue.nextTick(_ => {
          el.focus()
        })
      }
    }
  }

}
</script>

<style src="./todos.css"></style>
