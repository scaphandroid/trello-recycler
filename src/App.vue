<template>
  <div id="app">
    <Authorizer v-if="!authorized"/>
    <Recycler v-else/>
  </div>
</template>

<script>
import Recycler from './components/Recycler'
import Authorizer from './components/Authorizer'

export default {
  name: 'App',
  components: {
    Recycler,
    Authorizer
  },
  data () {
    return {
      key: '',
      token: '',
      authorized: false
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
