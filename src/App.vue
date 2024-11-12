<script>
import axios from 'axios';
import { computed } from 'vue';
export default {
  
  data() {
    return {
      city: '',
      error:'',
      info:null
      
    };
   
  },
  methods: {
    getWeather() {
        if (this.city.trim().length < 2) {
            this.error = 'Нужно название более одного символа';
            this.info = null; 
            return false;
        }

        this.error = ''; 

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=733ad0147444577d6efd696cb7d20631`)
            .then(res => {
                    this.info = res.data; 
            })
            .catch(err => {
                this.error = 'Город не найден';
                console.error(err);
                this.info = null; 
            });
            
    }
  },
  computed:{
    showTemp() {
    return "Температура:" + this.info.main.temp.toFixed(0)
    },
    showFeelsLike() {
    return "Ощущается как: " + this.info.main.feels_like.toFixed(0)
    },
    showMinTemp() {
    return "Минимальная температура:" + this.info.main.temp_min.toFixed(0)
    },
    showMaxTemp(){
    return "Максимальная температура: "+ this.info.main.temp_max.toFixed(0)
  }
}
}
</script>

<template>
 <div className ="wrapper">
    <h1>Погодное приложение </h1>
    <p v-if="city">Узнать погоду в {{ city }}</p>
    <p v-else>Узнать погоду в вашем городе</p>
    <input type="text" @:input="city = $event.target.value" placeholder="Введите город">
    <button v-if="city !=''" @click="getWeather(city)">Получить погоду</button>
    <button disabled v-else id="myButton">Получить погоду</button>
    <div v-if="info != null">
      <p>{{showTemp}}°C</p>
      <p>{{showFeelsLike}}°C</p>
      <p>{{ showMinTemp}}°C</p>
      <p>{{ showMaxTemp }}°C</p>
    </div>
    <div v-else>
      <p className="error">{{ error }}</p>
    </div>
    
 </div>
</template>

<style scoped>
.error{
  color: red;
}
.wrapper{
    margin: 0px 20px 0px 20px;
    width: 900px;
    height: 500px;
    min-width: 300px;
    border-radius: 50px;
    background: #1f0F24;
    object-fit: fill;
    text-align: center;
    color: #fff;
    font-family: Trebuchet MS, sans-serif;

}
.wrapper h1{
    margin-top: 20px;
}
.wrapper p{
    margin-top: 20px;
    margin-bottom: 20px;
    font-size:larger;
}
.wrapper input{
  font-size: x-large;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #110813;
    color: #fcfcfc;
    font-size: 5px 8px;
    outline: none;
}
.wrapper input :focus{
    border-bottom-color: #6e2d7d ;
}
.wrapper button :disabled {
    background: #746027;
        opacity: 0.5;
    cursor: not-allowed;
}
.wrapper button {
    font-size:larger;
    background: #e3bc4b;
    color:#fff;
    border-radius: 10px;
    border: 2px solid #b99935;
    padding: 10px 15px;
    margin-left:10px;
    cursor: pointer;
    transition: transform 500ms ease;
}
.wrapper button:hover {
    transform: scale(1.01) translateY(-5px);
}
@keyframes shake {
    0% { transform: translateX(0); }
    25% { transform: translateX(-5px); }
    50% { transform: translateX(5px); }
    75% { transform: translateX(-5px); }
    100% { transform: translateX(0); }
}
.wrapper button:disabled:active {
   animation: shake 0.1s infinite;
}
@media screen and (max-width: 900px) {
  input{
    text-align: center;
    width: 200px;
    font-size: x-large;
    
  }
  bottom{
    width: 300px;
  }
  p{
    font-size:x-large;
  }
  .wrapper {
    display: flex;
    flex-direction: column;
    flex-wrap: nowrap;
   justify-content: space-evenly;
    align-items: stretch;
    width: 900px;
    height: 500px;
    min-width: 300px;
    border-radius: 50px;
    background: #1f0F24;
    object-fit: fill;
    text-align: center;
    color: #fff;
    font-family: Trebuchet MS, sans-serif;
    width: 100%;
    padding: 1.5rem;
     align-items: center;
  }
  .wrapper button {
    background: #e3bc4b;
    color:#fff;
    border-radius: 10px;
    border: 2px solid #b99935;
    padding: 10px 15px;
    margin-left:0px;
    cursor: pointer;
    transition: transform 100ms ease;
    }

    .wrapper button:active {
        transform: scale(0.85)  translateY(-5px); 
    }
}
</style>
