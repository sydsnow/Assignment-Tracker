<script>
import Assignments from './assignments.json'; 

export default {
  data() {
    return {
      assignments: Assignments, 
      hideCompleted: false,
      title: 'Assignment Tracker'
    };
  },

  methods: {

  },
  computed: {
    filteredAssignments() {
      return this.hideCompleted
        ? this.assignments.filter((a) => !a.done)
        : this.assignments;
    },
  },

  mounted() {
  },
};
</script>

<template>
  <h1>{{ title }}</h1>
  <ul>
    <li v-for="assignment in filteredAssignments" :key="assignment.id" id="assignment-container">
      <input type="checkbox" v-model="assignment.done" id="input"/>
      <span :class="{ done: assignment.done }">
        {{ assignment.title }}
        <ul>
          <li>Due: {{ assignment.dueDate }}</li>
          <li>Days Remaining: {{ assignment.daysRemaining }}</li>
          <li> Percent Finished: {{ assignment.percentFinished }}%</li>
        </ul>
      </span>
      <!-- <button @click="deleteTask(task)">X</button> -->
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? "Show All" : "Hide Completed" }}
  </button>
</template>