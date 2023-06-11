<template>
  <div class="container">
    <button class="new-project-button" @click="showForm = true">New Project</button>
    <ul class="project-list">
      <li v-for="project in projects" :key="project.id" class="project-item">
        <div class="project-details">
          <div class="customer-name">{{ project.customerName }}</div>
          <div class="project-name">{{ project.projectName }}</div>
          <div class="time-ago">{{ project.timeAgo }}</div>
        </div>
      </li>
    </ul>

    <div class="form-modal" v-if="showForm">
      <div class="form-modal-content">
        <h2>Create New Project</h2>
        <form @submit.prevent="createProject">
          <label for="customerName">Customer Name:</label>
          <input type="text" id="customerName" v-model="newProject.customerName" required>

          <label for="projectName">Project Name:</label>
          <input type="text" id="projectName" v-model="newProject.projectName" required>

          <button type="submit">Create</button>
        </form>
        <button class="close-button" @click="showForm = false">Close</button>
      </div>
    </div>
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
      newProject: {
        customerName: '',
        projectName: '',
      },
      showForm: false,
    };
  },
  methods: {
    createProject() {
      const newProject = {
        id: this.projectIdCounter,
        customerName: this.newProject.customerName,
        projectName: this.newProject.projectName,
        createdAt: new Date(),
      };
      this.projects.unshift(newProject);
      this.projectIdCounter++;
      this.newProject.customerName = '';
      this.newProject.projectName = '';
      this.showForm = false;
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

.form-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.form-modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  text-align: center;
}

.form-modal-content h2 {
  margin-top: 0;
}

.form-modal-content form {
  display: flex;
  flex-direction: column;
}

.form-modal-content label {
  margin-top: 10px;
}

.form-modal-content input {
  padding: 5px;
  border-radius: 3px;
  border: 1px solid #ccc;
  margin-top: 5px;
}

.form-modal-content button[type="submit"] {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  margin-top: 10px;
}

.close-button {
  margin-top: 10px;
  background-color: #ccc;
  color: white;
  border: none;
  padding: 5px 10px;
  font-size: 14px;
  cursor: pointer;
}
</style>

