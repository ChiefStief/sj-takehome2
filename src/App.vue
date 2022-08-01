<script setup>
import QuestionJSON from "./questions.json";
import Workspace from "./components/Workspace.vue";
</script>

<template>
  <body v-if="QuestionJSON" class="app-wrapper">
  <header class="app-header">Quiz Application</header>
  <Workspace :questions="QuestionJSON.questions" />
  </body>
</template>

<style scoped>
.app-wrapper {
  background-color: #f7f7f7;
}

.app-header {
  background-color: #7c0200;
  color: white;
  padding: 5px;
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 10;
}
</style>

