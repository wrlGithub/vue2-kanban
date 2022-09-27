<template>
  <div id="app">
    <BarNav></BarNav>
    <KanBan></KanBan>
  </div>
</template>

<script>
import BarNav from './components/barNav.vue'
import KanBan from './components/kanBan.vue'
export default {
  name: 'App',
  components: {
    BarNav,
    KanBan
}
}
</script>

<style>
html,body {
  height: 100%;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  height: 100%;
}
.kanban{
  flex: 1;
}

</style>
