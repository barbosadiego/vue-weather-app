<template>
  <div id="app">
    <div class="overlay"></div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  }
}
</script>

<style>
*{
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}
#app{
  width: 100%;
  height: 100vh;
  background-image: url(./assets/background.jpg);
  background-size: cover;
  background-position: center bottom;
  display: grid;
}
.overlay{
  width: 100%;
  height: 100vh;
  background-color: rgba(0, 0, 0, .5);
  grid-column: 1;
}
</style>
