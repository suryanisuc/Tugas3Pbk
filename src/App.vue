<template>
  <div class="background">
    <background />
  </div>
  <div class="content container mt-5">
    <h1>Daftar Kegiatan</h1>
    <AddTask @task-added="addTask" />
    <div class="filter my-3">
      <button class="btn btn-primary" @click="showFilterPopup">Filter</button>
      <button class="btn btn-danger" v-if="showFilter" @click="clearFilter">Hapus Filter</button>
    </div>
    <table class="table table-hover">
      <thead class="table-dark">
        <tr>
          <th>Kegiatan</th>
          <th>Status</th>
          <th>Aksi</th>
        </tr>
      </thead>
      <tbody>
        <TaskItem v-for="(task, index) in filteredTasks" :key="index" :task="task" 
                  @update-task="updateTask" @cancel-task="cancelTask" 
                  @complete-task="completeTask" @delete-task="deleteTask" />
      </tbody>
    </table>
    <div v-if="showPopup" class="popup position-fixed top-50 start-50 translate-middle">
      <h2 class="popup-title">Filter</h2> 
      <div class="button-container d-flex justify-content-around">
        <button @click="filterByStatus(true)" class="btn btn-success">Selesai</button>
        <button @click="filterByStatus(false)" class="btn btn-warning">Belum Selesai</button>
      </div>
      <button class="btn-close position-absolute top-0 end-0" @click="showPopup = false"></button>
    </div>
  </div>
</template>


<script>
import { ref, computed } from 'vue';
import AddTask from './components/AddTask.vue';
import TaskItem from './components/TaskItem.vue';
import background from './components/background.vue';

export default {
  components: {
    AddTask,
    TaskItem,
    background
  },
  setup() {
    const tasks = ref([
      { id: 1, title: 'Mengerjakan Tugas', completed: false },
      { id: 2, title: 'Menyapu Rumah', completed: true },
      { id: 3, title: 'Bersepeda', completed: false }
    ]);
    const showPopup = ref(false);
    const showFilter = ref(false);
    const filterStatus = ref(null);

    const filteredTasks = computed(() => {
      return filterStatus.value === null ? tasks.value : tasks.value.filter(task => task.completed === filterStatus.value);
    });

    function addTask(newTask) {
      tasks.value.push({ ...newTask, id: tasks.value.length + 1, completed: false });
    }

    function updateTask(updatedTask) {
      const index = tasks.value.findIndex(task => task.id === updatedTask.id);
      if (index !== -1) {
        tasks.value[index] = {...tasks.value[index], ...updatedTask};
      }
    }

    function cancelTask(taskId) {
      const task = tasks.value.find(task => task.id === taskId);
      if (task) {
        task.completed = false;
      }
    }

    function completeTask(taskId) {
      const task = tasks.value.find(task => task.id === taskId);
      if (task) {
        task.completed = true;
      }
    }

    function deleteTask(taskId) {
      const index = tasks.value.findIndex(task => task.id === taskId);
      if (index !== -1) {
        tasks.value.splice(index, 1);
      }
    }

    function showFilterPopup() {
      showPopup.value = true;
    }

    function filterByStatus(completed) {
      filterStatus.value = completed;
      showPopup.value = false;
      showFilter.value = true;
    }

    function clearFilter() {
      filterStatus.value = null;
      showFilter.value = false;
    }

    return {
      tasks,
      filteredTasks,
      addTask,
      updateTask,
      cancelTask,
      completeTask,
      deleteTask,
      showPopup,
      showFilter,
      showFilterPopup,
      filterByStatus,
      clearFilter
    };
  }
};
</script>

<style scoped>
.content {
  position: fixed;
  top: 15%;
  left: 10%;
  align-items: center;
  justify-content: center;
}
.close:hover::before {
  content: ""; 
  position: absolute; 
  top: 0px;
  left: -5px; 
  right: -5px;
  bottom: -5px; 
  background-color: red;
  z-index: -1; 
  border-radius: 0px; 
}

table {
  width: 50%;
  margin: 20px auto;
  border-collapse: collapse;
  border-spacing: 0;
  position: fixed;
  backdrop-filter: blur(3px);
  
}

th, td {
  border: 1px solid black; 
  padding: 8px; 
  text-align: center; 
}

th {
  font-weight: bolder;
  background-color: gray; 
}

.popup {
  position: fixed;
  top: 30%; 
  left: 50%; 
  transform: translate(-50%, 0); 
  background-color: transparent; 
  border: 1px solid gray; 
  backdrop-filter: blur(5px);
  padding: 20px; 
  z-index: 1000; 
  display: flex; 
  flex-direction: column;
  align-items: center; 
  justify-content: flex-start;
  width: 300px; 
  height: 200px;
}

.button-container {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}

.popup-title {
  margin-bottom: 10px; 
  font-size: 20px; 
  text-align: center; 
  color: black;
  font-weight: bolder;
}

.popup-button {
  padding: 10px 20px;
  margin: 10px; 
  border-radius: 20px;
  border-color: transparent;
  margin-top: 10%;
}

.action-button.cancel {
  background-color: grey;
  color: white; 
  border-color: transparent;
}

.action-button.complete {
  background-color: blue; 
  color: white; 
  border-color: transparent;
}

.action-button.delete {
  background-color: red;
  color: white;
  border-color: transparent;
}

.popup-button.complete {
  background-color: blue;  
  color: white;
  border-color: transparent;
}

.close {
  position: absolute; 
  top: 0px; 
  right: 5px; 
  cursor: pointer; 
  color: red;
}
.close:hover {
  color: white; 
}

.action-button {
  padding: 8px 16px;
  margin: 4px;
}

.filter button {
  background-color: aquamarine;
  border: none; 
}
</style>