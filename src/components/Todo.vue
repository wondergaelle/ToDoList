<template>
  <section class="todoapp container">
    <header class="header">
      <h1>To Do List</h1>
      <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
    </header>

    <div class="main">
      <ul class="todo-list">
        <li class="todo" v-for="todo in todos">
          <div class="view">
            <label >{{todo.name}}</label>
          </div>
        </li>
      </ul>
    </div>

  </section>

</template>

<script>
export default {
  data(){
    return{
      todos:[{
        name: 'tâche de test',
        completed:false
      }],
      newTodo:''
    }
  },
  methods:{
    addTodo(){
      this.todos.push({
        completed: false,
        name: this.newTodo
      })
      this.newTodo = ''
    }
  }
}
</script>

<style src="./todo.css"></style>