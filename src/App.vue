<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="scss">
@import './assets/scss/variables';

#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: $color-gray;
  min-height: 100vh;
  background: $color-secondary;
  display: flex;
  flex-direction: column;
}

html,
body {
  margin: 0;
  width: 100vw;
  min-height: 100vh;
}

</style>
