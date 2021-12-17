<template>
  <div v-show="showAddTask">
  <AddTask @add-task="addtask" :edittask="task" :editFormShow="editForm" @update-task="updateTask" />
  </div>
  <Tasks :tasks="tasks" @delete-task="deleteTask" @toggle-reminder="updateReminder" @edit-task="editTask" />
</template>

<script>
import AddTask from '../components/AddTask.vue'
import Tasks from '../components/Tasks.vue'
export default {
  name: 'Home',
  props: {
    showAddTask: Boolean,
    editForm: Boolean
  },
  components: {
    AddTask,
    Tasks
  },
  data () {
    return {
      tasks: [],
      task: []
    }
  },
  methods: {
    async addtask (task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(task)
      })
      const data = await res.json()
      this.tasks = [...this.tasks, data]
    },
    async fetchTasks () {
      const res = await fetch('api/tasks')
      const data = await res.json()
      return data
    },
    async fetchTask (id) {
      const res = await fetch(`api/tasks/${id}`)
      const data = await res.json()
      return data
    },
    async deleteTask (id) {
      const res = await fetch(`api/tasks/${id}`, {
        method: 'DELETE'
      })
      res.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Error Deleting')
    },
    async updateReminder (id) {
      const toggleReminder = await this.fetchTask(id)
      const updtReminder = { ...toggleReminder, reminder: !toggleReminder.reminder }
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(updtReminder)
      })
      const data = await res.json()
      this.tasks = this.tasks.map((task) => task.id === id ? { ...task, reminder: data.reminder } : task)
    },
    editTask (task) {
      this.$emit('show-task-edit', 'edit')
      this.task = task
    },
    async updateTask (task) {
      const id = task.id
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
          'x-access-token': 'token-value'
        },
        body: JSON.stringify(task)
      })
      if (!res.ok) {
        const message = `An error has occured: ${res.status} - ${res.statusText}`
        throw new Error(message)
      }
      const data = await res.json()
      this.tasks = this.tasks.map((value) => value.id === id ? { ...value, ...data } : value)
      this.$emit('hide-edit-task')
    }
  },
  async created () {
    this.tasks = await this.fetchTasks()
  },
  emits: ['show-task-edit', 'hide-edit-task']
}
</script>
