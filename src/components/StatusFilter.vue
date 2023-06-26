<script>
export default {
  props: {
    value: String,
  },

  emits: ['change'],
};
</script>

<template>
  <nav class="filter">
    <a
      href="#/"
      class="filter__link"
      :class="{ selected: value === 'all'}"
      @click="$emit('change', 'all')"
    >
      All
    </a>

    <a
      href="#/active"
      class="filter__link"
      :class="{ selected: value === 'active'}"
      @click="$emit('change', 'active')"
    >
      Active
    </a>

    <a
      href="#/completed"
      class="filter__link"
      :class="{ selected: value === 'completed'}"
      @click="$emit('change', 'completed')"
    >
      Completed
    </a>
  </nav>
</template>

<style>
</style>
