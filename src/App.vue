<template>
  <div id="app">
    <vue-draggable-tags></vue-draggable-tags>
  </div>
</template>

<script>
import VueDraggableTags from './components/vue-draggable-tags'

export default {
  name: 'app',
  components: {
    VueDraggableTags
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
