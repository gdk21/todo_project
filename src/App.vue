<template>
  <div id="app">

    <todos v-for="n in range" ></todos>
    <button @click.prevent="addTodo">Add</button>
  </div>
</template>

<script>
import Todos from './components/Todos'
export default {

  data () {
    return {
      title: '',
      range:0,
      todos: [{

      }]
    }
  },
  components: {
    Todos
  },
  methods: {
    addTodo () {
      console.log('lel')
      this.range += 1;
      this.title = 'lel';
    }
  }
}
</script>

<style>

</style>
