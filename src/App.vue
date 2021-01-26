<template>
  <div id="app">
    <AdminIndex/>
  </div>
</template>

<script>
import Anima from "@/components/Anima";
import AdminIndex from "@/components/AdminIndex";

export default {
  name: 'App',
  components: {
    AdminIndex,

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
