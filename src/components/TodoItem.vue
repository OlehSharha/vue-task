<script>
export default {
  name: 'TodoItem',

  props: {
    todo: Object,
  },

  emits: ['update', 'remove'],

  data() {
    return {
      editing: false,
      newTitle: this.todo.title,
    };
  },

  methods: {
    toggle() {
      this.$emit('update', {
        ...this.todo, completed: !this.todo.completed,
      });
    },

    rename() {
      if (!this.editing) {
        return;
      }

      this.editing = false;
      this.$emit('update', {
        ...this.todo, title: this.newTitle,
      });

      this.newTitle = '';
    },

    removeTodos() {
      this.$emit('remove');
    },

    edit() {
      this.newTitle = this.todo.title;
      this.editing = true;
      this.$nextTick(() => {
        this.$refs['title-field'].focus();
      });
    },
  },
};
</script>

<template>
 <div
  class="todo"
  :class="{completed: todo.completed}"
 >
  <label class="todo__status-label">
    <input
      type="checkbox"
      class="todo__status"
      :checked="todo.completed"
      @change="toggle"
    />
  </label>

  <form v-if="editing" @submit.prevent="rename">
    <input
      type="text"
      class="todo__title-field"
      placeholder="You can`t save empty todos"
      v-model.trim="newTitle"
      ref="title-field"
      @keyup.esc="editing = false"
      @blur="rename"
      required
    />
  </form>

  <template v-else>
    <span class="todo__title" @dblclick="edit" >{{ todo.title }}</span>

    <button class="todo__remove" @click="removeTodos">x</button>
  </template>
</div>
</template>

<script>
</script>
