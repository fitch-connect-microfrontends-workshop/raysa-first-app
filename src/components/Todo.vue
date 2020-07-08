

<template>
  <div class="container main" id="app">
    <h1>{{title}}</h1>
    <form class="form-inline form" action="">
      <input class="form-control" type="text" name="todo" v-model="todoText">
      <button class="btn btn-primary" v-on:click.prevent="createTodo()">Create</button> 
    </form>
    <br>
    <ul>
      <li v-for="(todo, index) in todos" v-bind:key="index">
        <input type="checkbox" v-model="todo.isDone" @change="saveToStore()">
        <label v-bind:class="{'checked': todo.isDone}" for="">{{todo.content}} {{todo.created|date}}</label>
        <button class="btn btn-danger" @click="deleteTodo(todo)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
import moment from 'moment';
moment.locale('en-nz');
export default {
  name: 'app',
  data() {
    return {
      title: 'Vuejs Todo App',
      todoText: '',
      todos: []
    }
  },
  created() {
    if (this.isInit) {
      this.todos = JSON.parse(localStorage.getItem('vues-todos'));
    }
  },
  computed: {
    isInit() {
      return localStorage.getItem('vues-todos') !== null;
    }
  },
  filters: {
    date(value) {
      return moment(value).calendar();
    }
  },
  methods: {
    createTodo() {
      if (this.todoText === '') {
        return;
      }
      this.todos.push({
        content: this.todoText,
        created: Date.now(),
        isDone: false
      });
      this.saveToStore();
      this.todoText = '';
    },
    saveToStore() {
      localStorage.setItem('vues-todos', JSON.stringify(this.todos));
    },
    deleteTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo);
      this.saveToStore();
    }
  }
}
</script>

<style scoped>
#app {
  padding: 50px;
}
.checked {
  text-decoration: line-through;  
}
.main {
    display: flex;
    flex-direction: column;
}
.form {
    margin: auto;
}
ul {
  list-style-type: none;
}
</style>

