<template>
  <div>
    <p v-if="error"><strong>Error - Try again</strong></p>
    <p><i>Please paste your Trello API Key and token</i></p>
    <div>
      <label for="apiKey">Your API Key</label>
      <input id="apiKey" type="text" v-model="credentials.key">
    </div>
    <div>
      <label for="token">Your Token</label>
      <input id="token" type="text" v-model="credentials.token">
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
      credentials: {
        key: '',
        token: ''
      }
    }
  },
  methods: {
    submitKeys: function () {
      this.error = false
      axios.get('https://api.trello.com/1/members/me/boards?key=' + this.credentials.key + '&token=' + this.credentials.token)
        .then((response) => {
          console.log(response)
          this.$emit('authorized', this.credentials)
        })
        .catch((error) => {
          console.log(error)
          this.error = true
        })
    }
  }
}
</script>
