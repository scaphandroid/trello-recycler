<template>
  <div>
    <p v-if="error"><strong>Error - Try again</strong></p>
    <p><i>Please paste your Trello API Key and token</i></p>
    <div>
      <label for="apiKey">Your API Key</label>
      <input id="apiKey" type="text" :value="key">
    </div>
    <div>
      <label for="token">Your Token</label>
      <input id="token" type="text" :value="token">
    </div>
    <button v-on:click="submitKeys">Submit</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Authorizer',
  data () {
    return {
      error: false,
      key: '',
      token: ''
    }
  },
  methods: {
    submitKeys: function () {
      this.error = false
      let scope = this
      axios.get('https://api.trello.com/1/members/me/boards?key=' + this.key + '&token=' + this.token)
        .then(function (response) {
          console.log(response)
        })
        .catch(function (error) {
          // handle error
          console.log(error)
          scope.error = true
        })
    }
  }
}
</script>
