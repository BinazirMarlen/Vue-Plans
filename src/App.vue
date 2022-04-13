<template>
  <nav class="container">
    <Header @toggle-add-task="toggleAddTask" title="Task Tracker" :showAddTask="showAddTask"/>
    <div v-show="showAddTask">
      <AddTask @add-task="addTask"/>
    </div>
    <Tasks
    @toggle-reminder = 'toggleReminder'
    @delete-task="deleteTask" :tasks="tasks" />
   
  </nav>
   
</template>

<script>
import Header from './components/Header.vue'
import Tasks from './components/Tasks.vue'
import AddTask from './components/AddTask.vue'

export default{
  name: 'App',
  components: {
    Header,
    Tasks, 
    AddTask
  },
  data() {
    return {
      tasks: [],
      showAddTask: false
    }
  },
 
  methods: {
    async deleteTask(id){
      const res = await fetch(`api/tasks/${id}`, {
        method : 'DELETE'
      })

      res.status ===200 ? (this.tasks = this.tasks.filter((task)=>task.id !==id)) : alert('error deleting task')
      
    },
    async toggleReminder(id) {
      const TaskToToggle = await this.fetchTask(id)
      const updateTask = {...TaskToToggle, reminder: !TaskToToggle.reminder}

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updateTask)
      })

      const data = await res.json()
      this.tasks = this.tasks.map((task)=> task.id === id? {...task, reminder: data.reminder} : task)
    },
    async addTask(task) {
      const res =await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(task)
      })
      const data = await res.json()
      this.tasks=[...this.tasks, data]
    },
    toggleAddTask() {
      this.showAddTask = !this.showAddTask
    },
    async fetchTasks() {
      const res = await fetch('api/tasks')
      const data = await res.json()
      return data
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`)
      const data = await res.json()
      return data
    },
  },
 
  async created() {
  this.tasks = await this.fetchTasks();
  }
}
</script>


<style>
.container{
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn{
  display: inline-block;
  background: black;
  color: #fff;
  border: none;
  padding: 10px 25px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
}
.btn:focus {
  outline: none;
  background: grey;
  color: black;
}
</style>
