<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/> -->
          <div class="main-content">
            
            <article class="widget">
                <label class="switch">
                    <input type="checkbox" onchange="changeTempIndex({{temp_c}})">
                    <span class="slider round"></span>
                </label>
                <form @submit.prevent="fetchData" class="weatherLocation">
                  <input v-model="destination" placeholder="Enter location name" type = "text"/><button>Seach</button>
                </form>
                <div class="weatherIcon">
                    <img v-bind:src= icon alt="weather">
                </div>
                
                <div class="weatherInfo">
                    <div class="temperature"><span>{{temp_c}}°C</span></div>
                    <div class="description">    
                        <div class="weatherCondition">{{condition}}</div>    
                        <div class="place">
                            {{place}}, {{region}}, {{country}}
                        </div>
                    </div>
                </div>
                <div class="date">{{ dateBuilder()}}</div>
            </article>
        </div>
</template>

<script>
import axios from 'axios';
//const KELVIN = 273

export default{

  data() {
    return {
      destination: '',
      query: 'london',
      key : '82005d27a116c2880c8f0fcb866998a0',
  
      place: '',
      region: '',
      country: '',
      temp_c: '',
      temp_f: '',
      condition: '',
      icon: '',
    }
  },

  
  methods:{

    onInput(e){
      this.destination = e.target.value;
    },

    dateBuilder () {
      
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      return new Date().getDate() + ", " + months[new Date().getMonth()].substring(0, 3);

    },


    fetchData() {

      // https://www.digitalocean.com/community/tutorials/vuejs-rest-api-axios

      let config = {
        params: {q: this.destination, days: '30'},
        headers: {
          'X-RapidAPI-Host': 'weatherapi-com.p.rapidapi.com',
          'X-RapidAPI-Key': 'ee63f3b69emshb06b9bf5709e36fp1eb79cjsn083704c21ec0'
        }
      }

      axios
        .get('https://weatherapi-com.p.rapidapi.com/forecast.json', config)
        .then(response=>{
            this.place = response.data.location.name,
            this.region = response.data.location.region,
            this.country = response.data.location.country,
            this.temp_c= response.data.current.temp_c,
            this.temp_f= response.data.current.temp_f,
            this.condition = response.data.current.condition.text,
            this.icon = response.data.current.condition.icon
            //console.log(response.data.location)
        }).catch(error =>{
          console.log(error)
        })
    },

  }

}

</script>

<style>

body {
  color: #333333;
  font-family: Arial, Helvetica, sans-serif;
  max-width: 650px;
  margin: 0 auto;
  padding: 0 16px;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main-content {
  flex-grow: 1;
}

header {
  margin-top: 16px;
  margin-bottom: 48px;
}

h2 {
  font-size: 64px;
  margin-bottom: 16px;
}


.widget {
  position: absolute;
  top: 50%;
  left: 50%;
  display: flex;
  height: 300px;
  width: 600px;
  transform: translate(-50%, -50%);
  flex-wrap: wrap;
  cursor: pointer;
  border-radius: 20px;
  box-shadow: 0 27px 55px 0 rgba(0, 0, 0, 0.3), 0 17px 17px 0 rgba(0, 0, 0, 0.15);
}

.weatherLocation{
  display: flex;
  align-items: center;
  width: 100%;
  justify-content: center;
  padding: 2rem;
  background-color: #fafafa;
}

.weatherLocation button{
  background: #080705;
  color:#fff;
  border: 1px solid transparent;
}
.weatherLocation input:focus{
  outline: 0 none;
}

.widget .weatherIcon {
  flex: 1 100%;
  height: 60%;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  background: #FAFAFA;
  font-family: weathericons;
  display: flex;
  align-items: center;
  justify-content: space-around;
  font-size: 100px;
}
.widget .weatherIcon i {
  padding-top: 30px;
}
.widget .weatherInfo {
  flex: 0 0 70%;
  height: 40%;
  background: #080705;
  border-bottom-left-radius: 20px;
  display: flex;
  align-items: center;
  color: white;
}
.widget .weatherInfo .temperature {
  flex: 0 0 40%;
  width: 100%;
  font-size: 35px;
  display: flex;
  justify-content: space-around;
}
.widget .weatherInfo .description {
  flex: 0 60%;
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  justify-content: center;
}
.widget .weatherInfo .description .weatherCondition {
  text-transform: uppercase;
  font-size: 20px;
  font-weight: 100;
}
.widget .weatherInfo .description .place {
  font-size: 15px;
}
.widget .date {
  flex: 0 0 30%;
  height: 40%;
  background: rgb(68, 112, 207);
  border-bottom-right-radius: 20px;
  display: flex;
  justify-content: space-around;
  align-items: center;
  color: white;
  font-size: 30px;
  font-weight: 800;
}

p {
  position: fixed;
  bottom: 0%;
  right: 2%;
}
p a {
  text-decoration: none;
  color: #E4D6A7;
  font-size: 10px;
}

/* Toogle Swithc functionality */
/* The switch - the box around the slider */
.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

/* Hide default HTML checkbox */
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

/* The slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #2196F3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
</style>
