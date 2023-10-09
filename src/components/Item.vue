<template>
  <li>
    <button @click.self="$emit('complete')" :class="className">
      <i class="far fa-circle"></i> {{ task.title }}
    </button> 
    <button @click="$emit('remove')">-</button>
  </li>
</template>

<script>
export default {
  name: "TaskItem",
  props: ["task"],
  computed: {
    className() {
      let classes = ['toggle'];
      if(this.task.completed) {
        classes.push('toggle-completed');
      }
      return classes.join(' ');
    }
  }
};
</script>
