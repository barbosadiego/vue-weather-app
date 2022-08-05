<template>
  <div id="app">

    <div class="overlay"></div>

    <main>

      <form>

        <input 
          class="search"
          type="text"
          placeholder="Digite o local e pressione Enter"
          v-model="location"
          @keydown.enter.prevent="getWeather"
        />

        <button @click.prevent="getWeather">Buscar</button>

      </form>
      
      <transition>

        <div class="info" v-if="responseData.base">
          <h1>{{responseData.name}}, {{responseData.sys.country}}</h1>
          <p class="data">{{diaAtual}}</p>
          <div class="temp">
            <p class="main-temp">{{Math.round(responseData.main.temp)}}ºC</p>
            <p class="secondary-temp"><span>Máx.</span> {{Math.round(responseData.main.temp_max)}}Cº</p>
            <p class="secondary-temp"><span>Min.</span> {{Math.round(responseData.main.temp_min)}}Cº</p>
          </div>
          <p class="description">{{responseData.weather[0].description}}</p>
          <p class="humidity">Humidade do ar {{responseData.main.humidity}}%</p>
          <div class="icon">
            <img :src="`http://openweathermap.org/img/wn/${responseData.weather[0].icon}@2x.png`">
          </div>
          <p>{{responseData.message}}</p>
        </div>

        <div v-else-if="responseData.message">
          <p class="error">Erro: localização não encontrada.</p>
        </div>


      </transition>

      <LoadingPage v-if="loading" />

    </main>

  </div>

</template>

<script>
import LoadingPage from '@/components/Loading.vue'

const now = new Date();
const daysArray = ['dom','seg','ter','qua','qui','sex','sab'];
const monthsArray = ['jan','fev','mar','abr','mai','jun','jul','ago','set','out','nov','dez'];

export default {
  name: 'App',
  data() {
    return {
      location: '',
      api: 'eb9f08d66580680f98b067338772bcb0',
      responseData: {},
      loading: false,
    };
  },
  components:{
    LoadingPage,
  },
  computed:{
    diaAtual(){
      const hoje = daysArray[now.getDay()];
      const diaMes = now.getDate();
      const mes = monthsArray[now.getMonth()];
      const ano = now.getFullYear();
      return `${hoje}, ${diaMes} de ${mes} de ${ano}, ${now.getHours()}:${now.getMinutes()}`
    }
  },
  methods:{
    getWeather(){
      this.responseData = {};
      if(this.location.length){
        this.loading = true;
        try{
          fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.location}&appid=${this.api}&units=metric&lang=pt_br`)
          .then(res => res.json())
          .then(json => {
            if(json.cod === 200) {
              this.loading = false;
              this.responseData = json;
            }
            else if(json.cod === '404') {
              this.loading = false;
              this.responseData = json;
            } 
          })
        }
        catch(error){
          console.log(error);
        }
      }
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
  min-height: 100vh;
  background-image: url(./assets/background.jpg);
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center bottom;
  display: grid;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
.overlay {
  width: 100%;
  min-height: 100vh;
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
  /* height: 100vh; */
  padding: 5rem;
}
form{
  display: grid;
  gap: 20px;
  justify-items: center;
}
form button{
  width: 100px;
  padding: 10px 15px;
  font-size: 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-bottom: 50px;
  transition: .3s;
}
form button:hover,
form button:active{
  box-shadow: 0 0 0 3px rgba(255, 255, 255, .6);
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
  background-color: rgba(255, 255, 255, .1);
  border-radius: 10px;
  padding: 15px 30px;
  display: grid;
  grid-template-columns: repeat(2,1fr);
  justify-items: center;
  gap: 20px;
}
.temp .main-temp{
  grid-row: 1;
  grid-column: 1/-1;
}
.temp .secondary-temp{
  font-size: 1.5rem;
  text-shadow: none;
}
.temp span{
  font-size: 1rem;
  grid-row: 2;
}
.humidity{
  font-size: 1.1rem;
}
.description{
  font-size: 1.5rem;
  text-align: center;
  text-transform: capitalize;
}
.data{
  text-transform: uppercase;
  font-weight: bold;
}
.error{
  color: white;
  font-size: 1rem;
}
.v-enter-active,
.v-leave-active{
  transition: opacity 3s;
}
.v-enter,
.v-leave-to{
  opacity: 1;
}
@media screen and (max-width:600px) {
  main{
    padding: 20px;
  }
  form{
    margin-top: 50px;
  }
  form button:hover{
    box-shadow: unset;
  }
  .search{
    font-size: 1.1rem;
  }
}
</style>
