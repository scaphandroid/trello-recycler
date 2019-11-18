<template>
  <div>
    <div v-if="!board.name">
      <p>You need to select a board</p>
    </div>
    <div v-else-if="!moving">
      <ul>
        <li v-for="list in lists">
          <label for="list.id">{{ list.name }}</label>
          <input :id="list.id" type="checkbox" :value="list.id" v-model="selectedLists">
        </li>
      </ul>
      <div>
        <label for="board-select">Select board to move the lists to</label>
        <select id="board-select" v-model="selectedBoard">
          <option v-for="board in boards" :value="board">{{board.name}}</option>
        </select>
      </div>
      <button v-if="selectedBoard.name && selectedLists.length > 0" v-on:click="startMoving">Move lists</button>
    </div>
    <div v-else>
      <p>Moving.. {{ selectedLists.length - movedListsNb}} lists remaning</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'listsMover',
  props: ['board', 'credentials', 'boards'],
  watch: {
    board: function () {
      this.getLists()
    }
  },
  mounted () {
    this.getLists()
  },
  data () {
    return {
      lists: [],
      selectedLists: [],
      selectedBoard: {},
      moving: false,
      movedListsNb: 0
    }
  },
  methods: {
    getLists: function () {
      axios.get('https://api.trello.com/1/boards/' + this.board.id + '/lists?key=' + this.credentials.key + '&token=' + this.credentials.token)
        .then((response) => {
          this.lists = response.data
        })
    },
    startMoving: async function () {
      this.moving = true
      for (let i = 0; i < this.selectedLists.length; i++) {
        await axios.put('https://api.trello.com/1/lists/' + this.selectedLists[i] + '/idBoard?value=' + this.selectedBoard.id + '&key=' + this.credentials.key + '&token=' + this.credentials.token)
          .then((response) => {
            this.movedListsNb++
            return true
          })
          .catch((error) => {
            alert(error)
            return false
          })
      }
    }
  }
}
</script>
