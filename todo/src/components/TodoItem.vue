<template>
  <div>
    {{ index + 1 }} - {{ todo.title }}
    <input type="checkbox" :checked="todo.completed" @change="complete" />
  </div>
</template>

<script>
export default {
  props: {
    todo: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    }
  },
  emits: ['complete-done'],
  methods: {
    complete() {
      this.$emit('complete-done');
    }
  }
};
</script>

<style scoped></style>
