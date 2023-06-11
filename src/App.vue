<template>
  <div>
    <button @click="addNewProject">New Project</button>
    <ul>
      <li v-for="project in projects" :key="project.id">
        {{ project.name }}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  data() {
    return {
      projects: [
        { id: 1, name: 'Project 1' },
        { id: 2, name: 'Project 2' },
        { id: 3, name: 'Project 3' },
      ],
      projectIdCounter: 4,
    };
  },
  methods: {
    addNewProject() {
      const newProject = {
        id: this.projectIdCounter,
        name: 'New Project ' + this.projectIdCounter,
      };
      this.projects.unshift(newProject);
      this.projectIdCounter++;
    },
  },
});
</script>
