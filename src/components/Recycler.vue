<template>
  <section>
    <header>
      <h1>Board Recycler</h1>
      <label for="board-select">Select board to clean</label>
      <select id="board-select" v-model="selectedBoard" v-on:change="onBoardSelected">
        <option v-for="board in boards" :value="board">{{board.name}}</option>
      </select>
      <div>
        <button disabled>Copy board</button>
        <button v-on:click="action = 'archive'">Archive all lists in board</button>
        <button disabled>Delete all archived in board</button>
        <button v-on:click="action = 'move'">Move lists from board to another</button>
      </div>
      <Archiver v-if="action === 'archive'" :board="selectedBoard" :credentials="credentials"/>
      <listsMover v-if="action === 'move'" :board="selectedBoard" :credentials="credentials" :boards="boards"/>
    </header>
  </section>
</template>

<script>
import axios from 'axios'
import Archiver from './Archiver'
import listsMover from './listsMover'
export default {
  name: 'Recycler',
  components: {
    Archiver,
    listsMover
  },
  props: ['credentials'],
  data () {
    return {
      boards: [],
      selectedBoard: {},
      action: ''
    }
  },
  mounted () {
    this.getBoards()
  },
  methods: {
    getBoards: function () {
      axios.get('https://api.trello.com/1/members/me/boards?key=' + this.credentials.key + '&token=' + this.credentials.token)
        .then((response) => {
          console.log(response)
          this.boards = response.data
        })
        .catch((error) => {
          alert(error)
        })
    },
    onBoardSelected: function () {
      console.log(this.selectedBoard.name)
    },
    deleteAllArchived: function () {
    }
  }
}
</script>
