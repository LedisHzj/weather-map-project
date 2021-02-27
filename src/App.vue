<template>
  <v-app id="app">
    <div id="nav">
      <v-toolbar color="#dfe6e9" dense flat>
        <router-link to="/">Weather Map</router-link> |
        <router-link to="/about">Temperature Chart</router-link>
      </v-toolbar>
    </div>
    <router-view />
  </v-app>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding-bottom: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
