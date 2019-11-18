<template>
  <div>
    <div v-if="!board.name">
      <p>You need to select a board</p>
    </div>
    <div v-else-if="!confirm">
      <p>Please confirm you want to archive all lists from board <a :href="board.url" target="_blank">{{board.name}}</a></p>
      <button v-on:click="getLists">Confirm</button>
    </div>
    <div v-else>
      please wait.. {{ this.lists.length - archivedListsNb}} lists remaining
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Archiver',
  props: ['board', 'credentials'],
  watch: {
    board: function () {
      this.confirm = false
      this.lists = []
    }
  },
  data () {
    return {
      confirm: false,
      lists: [],
      status: 'waiting',
      archivedListsNb: 0
    }
  },
  methods: {
    getLists: function () {
      this.confirm = true
      axios.get('https://api.trello.com/1/boards/' + this.board.id + '/lists?key=' + this.credentials.key + '&token=' + this.credentials.token)
        .then((response) => {
          this.lists = response.data
          this.archiveAllLists()
        })
        .catch((error) => {
          alert(error)
        })
    },
    archiveAllLists: function () {
      for (let i = 0; i < this.lists.length; i++) {
        let list = this.lists[i]
        axios.put('https://api.trello.com/1/lists/' + list.id + '/closed?value=true&key=' + this.credentials.key + '&token=' + this.credentials.token)
          .then((response) => {
            this.archivedListsNb++
          })
          .catch((error) => {
            alert(error)
          })
      }
    }
  }
}
</script>
