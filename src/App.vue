<template>
  <div id="app">
    <div class="overlay"></div>
    <main>
      <div>
        <input 
          class="search"
          type="text"
          placeholder="Digite o local e pressione Enter"
          v-model="location"
          @keydown.enter="getWeather"
        />
      </div>

      <div class="info" v-if="responseData">
        <h1>{{responseData.name}}, {{responseData.sys.country}}</h1>
        <p class="data">{{diaAtual}}</p>
        <p class="temp">{{Math.round(responseData.main.temp)}}ºC</p>
        <p class="description">{{responseData.weather[0].description}}</p>
        <div class="icon">
          <img :src="`http://openweathermap.org/img/wn/${responseData.weather[0].icon}@2x.png`">
        </div>
      </div>

    </main>
  </div>
</template>

<script>
const now = new Date();
const daysArray = ['dom','seg','ter','qua','qui','sex','sab'];
const monthsArray = ['jan','fev','mar','abr','mai','jun','jul','ago','set','out','nov','dez'];

export default {
  name: 'App',
  data() {
    return {
      location: '',
      api: 'eb9f08d66580680f98b067338772bcb0',
      responseData: null,
    };
  },
  computed:{
    diaAtual(){
      const hoje = daysArray[now.getDay()];
      const diaMes = now.getDate();
      const mes = monthsArray[now.getMonth()];
      const ano = now.getFullYear();
      return `${hoje}, ${diaMes} de ${mes} de ${ano}`
    }
  },
  methods:{
    getWeather(){
      fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.location}&appid=${this.api}&units=metric&lang=pt_br`)
        .then(res => res.json())
        .then(json => {
          this.responseData = json
          console.log(json)
        })
    }
  }
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
.search{
  padding: 10px 15px;
  width: 300px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  font-size: 1rem;
  border-radius: 15px 0px 15px 0px;
  border: none;
  outline: none;
  text-align: center;
  margin-bottom: 50px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
.info{
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}
.temp{
  font-size: 5rem;
  font-weight: 900;
  text-shadow: 4px 6px rgba(0, 0, 0, 0.2);
  background-color: rgba(255, 255, 255, .2);
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  padding: 15px 30px;
}
.description{
  font-size: 1.2rem;
  text-align: center;
  text-transform: capitalize;
}
.data{
  text-transform: uppercase;
}
</style>
