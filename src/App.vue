<template>
  <div id="app">
    <md-toolbar class="md-large md-dense">
      <div class="md-toolbar-row">
        <h3 class="md-title">Large</h3>
      </div>
    </md-toolbar>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;

  }
</style>
