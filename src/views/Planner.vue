<template>
  <task :tasks="tasks" @update:tasks="updateTasks"></task>
  <planner :tasks="tasks" @update:tasks="updateTasks"/>
</template>

<script>
import planner from '../components/Planning.vue'

export default {
  name: "Plan",
  components: {
    planner,
},
  data() {
    return {
      tasks: [
        {
          id: 1,
          title: "Book a flight",
          completed: true,
        },
        {
          id: 2,
          title: "Pack personal things",
          completed: true,
        },
        {
          id: 3,
          title: "Find a hotel",
          completed: false,
        },
        {
          id: 4,
          title: "Bla bla",
          completed: false,
        },
        {
          id: 5,
          title: "Posting pictures online",
          completed: false,
        },
      ],
    };
  },
  methods: {
    updateTasks(newTasks) {
      this.tasks = newTasks;
    },
  },
};
</script>