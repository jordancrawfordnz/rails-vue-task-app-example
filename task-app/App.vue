<template>
  <div>
    <h1>My ugly TODO app!</h1>

    <form v-on:submit.prevent="addNewTask">
      <input v-model="newTask" type="text" placeholder="Enter your new task!" />
      <input type="submit" value="Add task!" />
    </form>

    <ul>
      <li v-for="task in tasks">
        <input type="checkbox" v-model="task.completed" />
        {{task.description}}
      </li>
    </ul>
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    data: function() {
      return {
        tasks: [],
        newTask: ""
      }
    },
    methods: {
      addNewTask: function() {
        axios.post('http://localhost:3000/tasks', {
          description: this.newTask,
          completed: false
        }).then(response => {
          this.tasks.push(response.data);
          this.newTask = "";
        })
      }
    },
    mounted: function() {
      axios.get('http://localhost:3000/tasks').then(response => this.tasks = response.data);
    }
  }
</script>