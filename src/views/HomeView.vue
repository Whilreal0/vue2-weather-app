<template>
  <div class="pa-10" id="background" :class="typeof weather.main != 'undefined' && isDay ? 'day' : 'night' " >
    <v-layout row wrap>
      <v-flex xs12 md6>
        <v-container class="pa-5 pa-md-16 mt-0 mt-md-16 text-center" >
          
            <v-text-field id="textfield" v-model="searchCity" label="Search City" solo @keypress="getWeather"></v-text-field>
          
            <p class="text-center" v-if="cityFound">No City Found</p>
        <div>
          <div class="card" v-if="typeof weather.main != 'undefined' && visible">
            <h1 id="location">{{weather.name}}, {{weather.sys.country}}</h1>
            <span class="subtitle-2 font-weight-regular" id="date">{{dateBuilder()}}</span>
            <!-- <p id="coordinate">{{Math.round(weather.coord.lat)}}&deg;N , {{Math.round(weather.coord.lon)}}&deg;W</p> -->
          
            <p class="text-center">{{weather.weather[0].description}}</p>
            <span class="weather-temp px-2 display-4" :class="isDay ? 'weather-temp-day' : 'weather-temp-night'">{{Math.round(weather.main.temp)}}&deg;C</span>
           
  
          </div>
        </div>
        </v-container>
      </v-flex>
      <!-- right from big screen  -->
      <v-flex xs12 md6>
        <v-container class="pa-5 pa-md-16 mt-0 mt-md-16 mt-md-0 pt-0 justify-content" v-if="typeof weather.main != 'undefined' && visible" >
         <div class="right ">
          <h2 class="text-center pb-2">Weather Details</h2>
          <div class="d-flex justify-content right-container py-2" :class="isDay ? 'right-container-day': 'right-container-night'">

              <div class="details " >
              <ul>
                  <li class="title" id="wind-speed-label">Wind Speed</li>         
                  <li class="title" id="wind-gust-label">Wind Gust</li>
                  <li class="title" id="wind-deg-label">Wind Deg</li>
                  <li class="title" id="Clouds-label">Clouds</li>
                  <li class="title" id="Pressure-label">Pressure</li>
                  <li class="title" id="Humidity-label">Humidity</li>
                  <li class="title" id="Visibility-label">Visibility</li> 
                </ul>
              </div>

              <div class="value">
                  <ul>
                    <li class="title" >{{weather.wind.speed}} m/s</li>
                    <li class="title" > {{weather.wind.gust}} mph</li>
                    <li class="title" > {{weather.wind.deg}} &deg;</li>
                    <li class="title"> {{weather.clouds.all }} %</li>
                    <li class="title"> {{Math.round(weather.main.pressure)}} hPa</li>
                    <li class="title"> {{Math.round(weather.main.humidity)}} %</li>
                    <li class="title"> {{weather.visibility}} km</li>
                  </ul>
              </div>
          </div>
         </div>

        </v-container>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
export default {
name: 'Home',
data: ()=>({
  isDay: true,
  
  key : 'eec872c979e19e8e88b17d79ff0e5883',
  baseURL: 'https://api.openweathermap.org/data/2.5/',
  searchCity: '',
  weather:{},
  visible: true,
  cityFound:false,
}),
methods:{
  getWeather(e){
    if(e.key == "Enter"){
      fetch(`${this.baseURL}weather?q=${this.searchCity}&appid=${this.key}&units=metric`)
      .then(res =>{
        return res.json();
      }).then(this.setResults);
      
    }
  },
  setResults (results) {
      try{
      this.weather = results;
      const timeOfDay = results.weather[0].icon;
      if(timeOfDay.includes("n")){
        this.isDay = false;
        
        return 'day'
      }else{
        this.isDay = true
        
        
      }
      this.visible =true
      this.cityFound = false
      console.log(timeOfDay)
      }catch(error){
        console.log(error);
        this.cityFound = true
        this.visible = false
        
      }
      return this.setResults
    },
  dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
    
    
}
}
</script>

<style>
@import '../assets/custom';
</style>