<template>
  <v-container class="" fluid>
    <v-col cols="12">
        <v-text-field
        v-model="city"
        class="mt-5"
        solo
        label="Search....."
        prepend-inner-icon="mdi-magnify"
        @keyup.enter="getWeatherData"
        ></v-text-field>
    </v-col>
    <div v-if="weather.weather && weather.weather.length">
    <v-row >
      <v-col class="">
        <v-card class="transparent ml-10" elevation="0">
          <v-list-item class="pa-0 ma-0">
            <v-list-item-content>
                <div>
                  <v-img contain class="ml-n8" height="200" width="200" :src="`http://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`" />
                </div>
                
              <v-list-item-title class="white--text display-3 font-weight-medium">
                {{ weather.weather[0].main}}
              </v-list-item-title>
              <v-list-item-title class="white--text headline">{{ weather.name }}, {{ weather.sys.country }}</v-list-item-title>
              <v-list-item-title class="white--text headline">{{ days[new Date(weather.dt * 1000).getDay()] }}, {{ new Date(weather.dt * 1000).getDate() }} {{ months[new Date(weather.dt * 1000).getMonth()] }} </v-list-item-title>
              <v-list-item-title class="white--text display-3 font-weight-black my-5">{{ Math.round(weather.main.temp - 274) }}°C</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-card>
      </v-col>
      <v-col class="d-flex flex-row-reverse mt-5">
        <v-card class="transparent" elevation="0">
          <v-card v-for="(item,index) in otherData" :key="index"  class="transparent ma-5" elevation="0">
          <v-list-item class="pa-0 ma-0">
            <v-icon color="white" size=50 class="ma-5">{{item.icon}}</v-icon>
            <v-list-item-content>
              <v-list-item-title class="white--text ">{{item.title}}  </v-list-item-title>
              <v-list-item-title class="white--text display-1 font-weight-bold">{{item.statistics}}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-card>
        </v-card>
      </v-col>
    </v-row>

    <v-sheet class="justify-center transparent mt-15 mx-2" elevation="0">
      <v-slide-group
        v-model="model"
        show-arrows
        dark
      >
        <v-slide-item
          v-for="(daily,index) in dailyForecast"
          :key="index"
        >
          <v-card
            class="ma-6 transparent"
            height="250"
            width="200"
            elevation="10"
          >
          <v-card-title class="white--text title center--text">
            {{days[new Date(daily.dt * 1000).getDay()]}}
          </v-card-title>
          <v-img contain class="center " height="100" width="100" :src="`http://openweathermap.org/img/wn/${daily.weather[0].icon}@2x.png`" />
          <v-card-title class="white--text font-weight-bold justify-center">
            <span class="display-1 font-weight-bold">{{ Math.round(daily.temp.max - 274) }}°C | </span>
            <span class="title font-weight-bold grey--text"> {{ Math.round(daily.temp.min - 274) }}°C</span>
            </v-card-title>
          
          </v-card>
        </v-slide-item>
      </v-slide-group>

    </v-sheet>
    </div>
    <div v-else>
      <v-card height="700" elevation="0" class="transparent mt-10">
        <v-card-title class="justify-center">
          <v-icon color="white" size="400">mdi-alert-circle</v-icon>
        </v-card-title>
        <v-card-title class="justify-center display-2 white--text font-weight-bold">No data found</v-card-title>
        
        
      </v-card>
      
    </div>
  </v-container>
</template>

<script>
  export default {
    name: 'HelloWorld',
    props:['background'],

    data () {
      return {
        model: null,
        city: 'Bhilwara',
        api_key: '5d58c07735d7fe6727a84e9fdbcbd119',
        weather: {},
        otherData: [],
        dailyForecast: {},
        days:['Sunday','Monday','Tuesday','Wednesday','Thrusday','Friday','Saturday'],
        months: ['January','February','March','April','May','June','July','August','September','October','November','December'],
        day: null,
      }
      
    },
    mounted(){
      this.getWeatherData();
    },
    methods: {
      async getWeatherData(){
        await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.api_key}`).then(res => {
          return res.json();
        }).then(result => {
          this.weather = result
 
        });
        this.otherData = [
          {
            icon: "mdi-weather-hail",
            title: 'Humidity',
            statistics: this.weather.main.humidity + ' %'
          },
          {
            icon: 'mdi-weather-tornado',
            title: 'Air Pressure',
            statistics: this.weather.main.pressure + ' PS'
          },
          {
            icon: 'mdi-weather-windy',
            title: 'Wind Speed',
            statistics: this.weather.wind.speed + ' km/h'
          },
          ];
          fetch(`https://api.openweathermap.org/data/2.5/onecall?lat=${this.weather.coord.lat}&lon=${this.weather.coord.lon}&exclude={minutely}&appid=${this.api_key}`).then(data=>{
            return data.json();
          }).then(result=>{
            this.dailyForecast = result.daily;
            let date = new Date(result.daily[0].dt * 1000)
            this.day = date.getDay()
          })
          setInterval(function(){ window.location.reload() }, 300000);
      }
      
    },
  }
</script>