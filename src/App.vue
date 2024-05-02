<template>
  <main>
    <!-- heading -->
    <header>
      <img src="./assets/pinia-logo.svg" alt="pinia logo">
      <h1>Pinia Tasks</h1>
    </header>

    <!-- new task form -->
    <div class="new-task-form">
      <task-form />
    </div>

    <!-- filter -->
    <nav class="filter">
        <button @click="filter='all'">All Tasks</button>
        <button @click="filter='favs'">Fav Tasks</button>
    </nav>

      <!-- loading -->
      <div class="loading" v-if="loading">Loading tasks...</div>

    <div class="task-list" v-if="filter === 'all'">
      <p>You have {{ totalCount }} tasks left to do</p>
      <div v-for="task in tasks">
        <task-details :task="task" />
      </div>
    </div>
    <div class="task-list" v-if="filter === 'favs'">
      <p>You have {{ favCount }} task left to do</p>
      <div v-for="task in favs">
        <task-details :task="task" />
      </div>
    </div>

    <button @click="taskStore.$reset">reset state</button>

  </main>
</template>

<script>
import { ref } from 'vue';
import TaskDetails from './components/TaskDetail.vue'
import { useTaskStore } from './stores/TaskStore';
import TaskForm from './components/TaskForm.vue';
import { storeToRefs } from 'pinia';
  export default {
    components: { TaskDetails, TaskForm },
    setup () {
      const taskStore = useTaskStore()
      
      const { tasks, loading, favs, totalCount, favCount } = storeToRefs(taskStore)

      //fetch tasks
      taskStore.getTasks()
    
      const filter = ref('all')
      return { taskStore, filter, tasks, loading, favs, totalCount, favCount }
    }
  }
</script>

