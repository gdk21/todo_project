<template>
  <div>
    {{ todos }}
  <todos v-model="todos"></todos>
  <todos></todos>
  <todos></todos>
    <button @click.prevent="addTodo">Add</button>
  </div>
</template>

<script>
import Todos from './components/Todos'

export default {

  data () {
    return {
      todos: [{
        name: 'Demo',
        completed: true
      }]
    }
  },
  components: {
    Todos
  },
  methods: {
    addTodo () {
      this.todos.push({
        name: 'Gael',
        completed: false
      })
    },
  }
}
</script>

<style>

</style>
