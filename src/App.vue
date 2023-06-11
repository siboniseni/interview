<template>
  <div class="container">
    <button class="new-project-button" @click="addNewProject">New Project</button>
    <ul class="project-list">
      <li v-for="project in projects" :key="project.id" class="project-item">
        <div class="project-details">
          <div class="customer-name">{{ project.customerName }}</div>
          <div class="project-name">{{ project.projectName }}</div>
          <div class="time-ago">{{ project.timeAgo }}</div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import moment from 'moment';

export default defineComponent({
  data() {
    return {
      projects: [
        { id: 1, customerName: 'John Doe', projectName: 'Project 1', createdAt: new Date() },
        { id: 2, customerName: 'Jane Smith', projectName: 'Project 2', createdAt: new Date() },
        { id: 3, customerName: 'Alex Johnson', projectName: 'Project 3', createdAt: new Date() },
      ],
      projectIdCounter: 4,
    };
  },
  methods: {
    addNewProject() {
      const newProject = {
        id: this.projectIdCounter,
        customerName: 'New Customer',
        projectName: 'New Project ' + this.projectIdCounter,
        createdAt: new Date(),
      };
      this.projects.unshift(newProject);
      this.projectIdCounter++;
    },
    calculateTimeAgo(createdAt: Date) {
      return moment(createdAt).fromNow();
    },
  },
});
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.new-project-button {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

.project-list {
  list-style-type: none;
  padding: 0;
}

.project-item {
  margin-bottom: 20px;
  background-color: #f4f4f4;
  border-radius: 5px;
  padding: 10px;
}

.project-details {
  display: flex;
  justify-content: space-between;
}

.customer-name {
  font-weight: bold;
}

.project-name {
  margin-top: 5px;
}

.time-ago {
  color: gray;
  font-size: 12px;
}
</style>
