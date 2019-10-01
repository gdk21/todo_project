<template>
<section class="todoapp">
  <header class="header">
    <h1>{{ title }}</h1>
    <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
  </header>
  <div class="main">
    <input type="checkbox" class="toggle-all" v-model="allDone">
    <ul class="todo-list">
      <li class="todo"  v-for="todo in filteredTodos" :class="{completed: todo.completed, editing: todo === editing}">
        <div class="view">
          <input type="checkbox" v-model="todo.completed" class="toggle">
          <label  @dblclick="editTodo(todo)">{{ todo.name }}</label>
          <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
        </div>
        <input type="text" class="edit" @keyup.enter="doneEdit" v-focus="todo === editing" v-model="todo.name" @blur="doneEdit" @keyup.esc="cancelEdit">
      </li>
    </ul>
  </div>
  <footer class="footer" v-show="hasTodos">
    <span class="todo-count"><strong>{{ remaining }}</strong> Tâches à faire</span>
    <ul class="filters">
      <li>
        <a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a>
      </li>
      <li>
        <a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a>
      </li>
      <li>
        <a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a>
      </li>
    </ul>
    <button class="clear-completed" @click.prevent="deleteCompleted" v-show="clearCompleted">Supprimer les tâches finies</button>
  </footer>
</section>

</template>

<script>
import Vue from 'vue'
export default {
  name: 'Todos',
  props: {
    value: { type: Array, default () { return [] } }
  },
  data () {
    return {
      title: '',
      todos: this.value,
      newTodo: '',
      filter: 'all',
      editing: null,
      oldTodo: ''
    }
  },
  watch: {
    value (value) {
      this.todo = value
    }
  },
  methods: {
    cancelEdit () {
      this.editing.name = this.oldTodo
      this.doneEdit()
    },
    editTodo (todo) {
      this.editing = todo
      this.oldTodo = todo.name
    },
    doneEdit () {
      this.editing = null
    },
    deleteCompleted () {
      this.todos = this.todos.filter(todo => !todo.completed)
      this.$emit('input', this.todos)
    },
    addTodo () {
      this.todos.push({
        name: this.newTodo,
        completed: false
      })
      this.newTodo = ''
    },
    deleteTodo (todo) {
      this.todos = this.todos.filter(i => i !== todo)
    }
  },
  computed: {
    clearCompleted () {
      return this.todos.filter(todo => !todo.completed)
    },
    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    },
    filteredTodos () {
      if (this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed)
      } else {
        return this.todos
      }
    },
    allDone: {
      get () {
        this.remaining === 0
      },
      set (value) {
        this.todos.forEach(todo => {
          todo.completed = value
        })
      }
    },
    hasTodos () {
      return this.todos.length > 0
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
<style src="./todos.css">

</style>
