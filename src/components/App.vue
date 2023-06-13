<template>
  <div class="container">
    <h1 class="title">Projects</h1>
    <div class="search-container">
      <button v-if="!showSearchForm" class="search-button" @click="showSearchForm = true">
        <i class="fa-solid fa-magnifying-glass"></i>Search</button>
      <div class="search-form" v-show="showSearchForm">
        <form @submit.prevent="filterProjects">
          <input type="text" v-model="searchText" placeholder="Search by name or project name"/>
          <button type="submit">Search</button>
        </form>
      </div>
    </div>
    <div class="project-list">
      <div v-for="project in projects" :key="project.id" class="project-item" @mouseover="highlightProject(project.id)" @mouseout="resetProjectHighlight(project.id)">
        <div class="project-details">
          <div class="customer-name">{{ project.customerName }}</div>
          <div class="project-name">{{ project.projectName }}</div>
          <div class="time-ago">{{ calculateTimeAgo(project.createdAt) }}</div>
        </div>
      </div>
    </div>
    
    <div class="new-project-container">
      <button class="new-project-button" @click="showForm = true">New Project</button>
    </div>

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

    <footer class="footer">
      <div class="footer-icons">
        <div class="footer-icon">
          <i class="fa-regular fa-circle-dot"></i>
          <span>Projects</span>
        </div>
        <div class="footer-icon">
          <i class="fa-solid fa-gear"></i>
          <span>Customers</span>
        </div>
        <div class="footer-icon">
          <i class="fa-regular fa-circle-dot"></i>
          <span>Company</span>
        </div>
      </div>
    </footer>
  
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import moment from 'moment';
import '@fortawesome/fontawesome-free/css/all.css';

interface Project {
  id: number;
  customerName: string;
  projectName: string;
  createdAt: Date;
}

export default defineComponent({
  data() {
    return {
      projects: [
        { id: 1, customerName: 'John Allen Cena', projectName: 'Project name', createdAt: new Date() },
        { id: 2, customerName: 'Jane W Smith', projectName: 'Project name', createdAt: new Date() },
        { id: 3, customerName: 'Alex Johnson', projectName: 'Project name', createdAt: new Date() },
        { id: 4, customerName: 'Andile Jali', projectName: 'Project name', createdAt: new Date() },
        { id: 3, customerName: 'Kasa Siboniseni', projectName: 'Project name', createdAt: new Date() },
        { id: 4, customerName: 'Athanathi K', projectName: 'Project name', createdAt: new Date() },
      ] as Project[],
      projectIdCounter: 5,
      newProject: {
        customerName: '',
        projectName: '',
      } as Project,
      showForm: false,
      showSearchForm: false,
      searchText: '',
      highlightedProjectId: null as null | number,
    };
  },
  methods: {
    createProject() {
      const newProject: Project = {
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
    calculateTimeAgo(createdAt: Date): string {
      return moment(createdAt).fromNow();
    },
    filterProjects() {
      this.projects = this.projects.filter(
        (project) =>
          project.customerName.toLowerCase().includes(this.searchText.toLowerCase()) ||
          project.projectName.toLowerCase().includes(this.searchText.toLowerCase())
      );
      this.showSearchForm = false;
    },
    highlightProject(id: number) {
      this.highlightedProjectId = id;
    },
    resetProjectHighlight(id: number) {
      if (this.highlightedProjectId === id) {
        this.highlightedProjectId = null;
      }
    },
  },
});
</script>

<style scoped>
.container {
  max-width: 460px;
  height: auto;
  margin: 0 auto;
  padding: 20px;
  background: #ccc;
}

.title {
  font-size: 40px;
  margin-bottom: 10px;
  
}

.search-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
}

.search-button {
  background-color: #ffffff;
  color: rgb(33, 12, 228);
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  margin-left: 5px;
  flex-shrink: 0;
  border-radius: 20px;
}

@media (max-width: 600px) {
  .search-container {
    flex-direction: column;
    align-items: flex-start;
  }

  .search-container .search-button {
    margin-top: 10px;
  }
}

.new-project-button {
  background-color: #0b07f5;
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  border-radius: 20px;
  width: 500px;
}

.project-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.project-item {
  width: 200px;
  height: 230px;
  background-color: #f4f4f4;
  border-radius: 10px;
  padding: 10px;
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: background-color 0.3s ease;
}

.project-item:hover {
  background-color: #9db9ec;
}

.project-details {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.customer-name {
  margin-bottom: auto;
  color: #383737;
}

.project-info {
  display: flex;
  flex-direction: column;
}

.project-name {
  font-weight: bold;
  margin-bottom: 4.5cm;
  font-size: 20px;
}

.time-ago {
  color: gray;
  font-size: 12px;
}

.search-form {
  margin-top: 10px;
}

.search-form form {
  display: flex;
  align-items: center;
}

.search-form input[type="text"] {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.search-form button[type="submit"] {
  background-color: #1606f0;
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  margin-left: 10px;
  border-radius: 4px;
}

.new-project-container {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 999;
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
  z-index: 9999;
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
  background-color: #ccc;
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
  background-color: #4060ec;
  color: rgb(255, 254, 254);
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

.footer {
  background-color: #f4f4f4;
  padding: 10px;
  text-align: center;
  font-size: 14px;
}
.footer-icons {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.footer-icon {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.footer-icon i {
  margin-bottom: 5px;
}

.footer p {
  margin-top: 0;
  text-align: center;
  font-size: 14px;
}


</style>

