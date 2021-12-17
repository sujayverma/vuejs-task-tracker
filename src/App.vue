/* eslint-disable */
<template>
<div class="container">
  <Header @toggle-add-task="toggleAddTask" title="Task Tracker" :showAddTask="showAddTask" />
  <router-view :showAddTask="showAddTask" @show-task-edit="toggleAddTask" :editForm="editForm" @hide-edit-task="toggleAddTask"></router-view>
  <Footer />
</div>

</template>
<script>
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'App',
  components: {
    Header,
    Footer
  },
  data () {
    return {
      showAddTask: false,
      editForm: false
    }
  },
  methods: {
    toggleAddTask (status) {
      if (status === 'edit') {
        if (!this.showAddTask) {
          this.showAddTask = !this.showAddTask
          this.editForm = true
        }
      } else {
        this.showAddTask = !this.showAddTask
        this.editForm = false
      }
    }
  }
}

// created is used as hook. it's a lifecycle method. It get revoked when loading a component.
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
