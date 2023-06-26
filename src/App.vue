<script>

import StatusFilter from './components/StatusFilter.vue';

import TodoItem from './components/TodoItem.vue';

export default {

  components: {
    StatusFilter,
    TodoItem,
  },

  data() {
    let todos = [];
    const todosFromLocale = localStorage.getItem('todos') || '[]';

    try {
      todos = JSON.parse(todosFromLocale);
    } catch (e) {
      throw new Error('Cant parse todos');
    }

    return {
      todos,
      title: '',
      status: 'all',
    };
  },

  watch: {
    todos: {
      handler() {
        localStorage.setItem('todos', JSON.stringify(this.todos));
      },
    },
  },

  computed: {
    activeTodos() {
      return this.todos.filter((todo) => !todo.completed);
    },

    doneTodos() {
      return this.todos.filter((todo) => todo.completed);
    },

    visibleTodos() {
      switch (this.status) {
        case 'active':
          return this.activeTodos;
        case 'completed':
          return this.doneTodos;

        default:
          return this.todos;
      }
    },
  },

  methods: {
    handleSubmit() {
      const newTodo = {
        id: Date.now(),
        title: this.title,
        completed: false,
      };

      this.todos = [...this.todos, newTodo];

      this.title = '';
    },

    deleteTodos(index) {
      const updatedTodos = this.todos.filter((todo, i) => i !== index);
      this.todos = updatedTodos;
    },

    deleteAllCompleted() {
      const deleteCompletedTodos = this.todos.filter((todo) => !todo.completed);
      this.todos = deleteCompletedTodos;
    },
  },

};
</script>

<template>
  <div class="todoapp">
    <h1 class="todoapp__title">Todos</h1>

    <div class="todoapp__content">
      <header class="todoapp__header">

        <form @submit.prevent="handleSubmit">
          <input
            type="text"
            class="todoapp__new-todo"
            placeholder="What needs to be done?"
            v-model.trim="title"
            required
          />
        </form>
      </header>

      <section class="todoapp__main">
        <TodoItem v-for="todo of visibleTodos"
          :key="todo.id"
          :todo="todo"
          @update="Object.assign(todo, $event)"
          @remove ="deleteTodos(todos.indexOf(todo))"
          />
      </section>

      <footer class="todoapp__footer">
        <span class="todo-count">
         {{ activeTodos.length }} {{ `${activeTodos.length > 1 ? 'items': 'item' }` }} left to do
        </span>
        <span class="todo-count">
         {{ doneTodos.length }} {{ `${doneTodos.length > 1 ? 'items': 'item' }` }} have done
        </span>

        <StatusFilter
         :value="status"
          @change="status = $event"
        />

        <button
        v-if="doneTodos.length !== 0"
        class="todoapp__clear-completed"
        @click="deleteAllCompleted"
         >
          Clear completed
        </button>
      </footer>
    </div>
    </div>
  </template>

<style>
</style>
