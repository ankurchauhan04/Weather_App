<template>
  <v-app>
    <v-main app>
      <Weather/>
    </v-main>
  </v-app>
</template>

<script>
import Weather from './components/WeatherComponent';

export default {
  name: 'App',

  components: {
    Weather,
  },

  data: () => ({
    //
  }),
};
</script>

<style>
  #app {
  background: url('https://images.pexels.com/photos/125457/pexels-photo-125457.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940')
    no-repeat center center fixed !important;
  background-size: cover;
  transition: 0.5s;
  }
  main{
    background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
  }
  *{
    margin: 0;
    padding: 0;
  }

</style>