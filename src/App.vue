<template>
  <div id="app">
    <div class="overlay"></div>
    <main>
      <div class="search">
        <input
          type="text"
          placeholder="Type here and press Enter"
          v-model="location"
        />
      </div>

      <div class="info">
        <h1>Recife, PE</h1>
        <p>Quinta, 04 de agosto de 2022</p>
        <p class="temp">21ªC</p>
        <p class="description">Clouds</p>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      location: '',
    };
  },
};
</script>

<style>
* {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}
#app {
  width: 100%;
  height: 100vh;
  background-image: url(./assets/background.jpg);
  background-size: cover;
  background-position: center bottom;
  display: grid;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
.overlay {
  width: 100%;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  grid-column: 1;
  grid-row: 1;
}
main {
  grid-column: 1;
  grid-row: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  height: 100vh;
  padding: 5rem;
}
</style>
