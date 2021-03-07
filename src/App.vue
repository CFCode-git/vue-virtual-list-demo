<template>
  <div id="app">
    <router-link to="/">Home</router-link> |
    <router-link to="/page-a">PageA</router-link> |
    <router-link to="/page-b/index">PageB</router-link> |
    <router-link to="/virtualList">virtual list</router-link>
    <div>
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
