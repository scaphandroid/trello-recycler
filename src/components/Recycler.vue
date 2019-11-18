<template>
  <section>
    <header>
      <h1>Board Recycler</h1>
      <label for="board-select">Select board to clean</label>
      <select id="board-select">
        <option v-for="board in boards" :value="board.id">{{board.name}}</option>
      </select>
      <div>
        <button disabled>Copy board</button>
        <button>Archive all lists in board</button>
        <button>Delete all archived in board</button>
      </div>
      <div>
        <i>{{status}}</i>
      </div>
    </header>
  </section>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Recycler',
  data () {
    return {
      boards: [],
      status: '...',
      msg: 'Welcome to Your Vue.js App'
    }
  },
  methods: {
    getBoards: function () {
    },
    archiveAllLists: function () {
    },
    deleteAllArchived: function () {
    }
  }
}
</script>
