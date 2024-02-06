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

  computed: {
    filteredAssignments() {
      return this.hideCompleted
        ? this.assignments.filter((a) => !a.done)
        : this.assignments;
    },
  },

  methods: {
    calculateDaysRemaining(dueDate) {
      const today = new Date();
      const due = new Date(dueDate);
      const timeDiff = due.getTime() - today.getTime();
      const daysRemaining = Math.ceil(timeDiff / (1000 * 3600 * 24)); // Convert milliseconds to days
      return daysRemaining;
    },
  },
  
  mounted() {
    // dynamically add daysRemaining to each assignment
    this.assignments.forEach(assignment => {
      assignment.daysRemaining = this.calculateDaysRemaining(assignment.dueDate);
    });
  },
};

</script>

<template>
  <h1>{{ title }}</h1>
  <ul>
    <li v-for="assignment in filteredAssignments" :key="assignment.id" class="assignment-container">
      <input type="checkbox" v-model="assignment.done" name="assignment-done" class="input"/>
      <span :class="{ done: assignment.done }">
        {{ assignment.title }}
        <ul>
          <li>Due: {{ assignment.dueDate }}</li>
          <li>Days Remaining: {{ assignment.daysRemaining }}</li>
          <li> Percent Finished: {{ assignment.percentFinished }}%</li>
        </ul>
      </span>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted" id="completed-button">
    {{ hideCompleted ? "Show All" : "Hide Completed" }}
  </button>
</template>