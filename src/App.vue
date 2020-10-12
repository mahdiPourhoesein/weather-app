<template>
  <div id="app" v-bind:class="{'am-body' : yes , 'pm-body' : no}">
    <div class="container first">
        <input id="search" v-model="search" @keypress="fetchWeather" v-on:keyup.enter="Loading" type="text" placeholder="which city...?" class="input" />
    </div>
    <div class="loading" v-if="this.country == ''">
        <div class="circle"></div>
        <div class="circle"></div>
        <div class="circle"></div>
    </div> 
    <h1 id="error">Ooops! Some Thing Went Wrong!</h1>
    <div class="tablet-mobile" v-if="this.country != ''">
        <div class="img-container">
            <img v-if=" this.yes == true " src="./assets/morning.svg"> <img v-if=" this.yes == false " src="./assets/moon.svg">
        </div>
        <div class="clock-country" v-if="this.country != ''">
            <div class="clock">
                <p>{{this.time}}</p>
                <p class="am-pm"> {{this.amPm}} </p>
            </div>
            <div class="country-container" v-if="this.country != ''">
                <p>{{this.location.name}}</p>
                <p class="country">{{this.country}}</p>
            </div>
        </div>
        <div class="week" v-if="this.country != ''">
            <div>
                <p>{{this.week_day[this.today]}} :</p>
                <p class="deg">{{Math.round(this.week[0].temp.max)}}&deg;C</p>
                <img :src='`/dist/${this.week[0].weather[0].main}.svg`'>
            </div>
            <div>
                <p>{{this.week_day[this.today + this.number[0]]}} :</p>
                <p class="deg">{{Math.round(this.week[1].temp.max)}}&deg;C</p>
                <img :src='`/dist/${this.week[1].weather[0].main}.svg`'>
            </div>
            <div>
                <p>{{this.week_day[this.today + this.number[1]]}} :</p>
                <p class="deg">{{Math.round(this.week[2].temp.max)}}&deg;C</p>
                <img :src='`/dist/${this.week[2].weather[0].main}.svg`'>
            </div>
            <div>
                <p>{{this.week_day[this.today + this.number[2]]}} :</p>
                <p class="deg">{{Math.round(this.week[3].temp.max)}}&deg;C</p>
                <img :src='`/dist/${this.week[3].weather[0].main}.svg`'>
            </div>
            <div>
                <p>{{this.week_day[this.today + this.number[3]]}} :</p>
                <p class="deg">{{Math.round(this.week[4].temp.max)}}&deg;C</p>
                <img :src='`/dist/${this.week[4].weather[0].main}.svg`'>
            </div>
            <div>
                <p>{{this.week_day[this.today + this.number[4]]}} :</p>
                <p class="deg">{{Math.round(this.week[5].temp.max)}}&deg;C</p>
                <img :src='`/dist/${this.week[5].weather[0].main}.svg`'>
            </div>
            <div>
                <p>{{this.week_day[this.today + this.number[5]]}} :</p>
                <p class="deg">{{Math.round(this.week[6].temp.max)}}&deg;C</p>
                <img :src='`/dist/${this.week[6].weather[0].main}.svg`'>
            </div>
        </div>    
    </div>
    <div class="container second" v-bind:class="{'am-second' : yes , 'pm-second' : no}" v-if="this.country != ''">
        <div class="top">
            <div class="country-container"><p>{{this.location.name}}/</p><p class="country">{{this.country}}</p></div>
            <div class="sun-cloud"> <img v-if=" this.yes == true " src="./assets/morning.svg"> <img v-if=" this.yes == false " src="./assets/moon.svg"> </div>
            <div class="day-deg">
                <div class="result">
                    <p>Current Temp :</p>
                    <div class="deg-container"> 
                        <p class="deg">{{this.currentTemp}}&deg;C</p>
                        <img :src='`/dist/${this.weather.current.weather[0].main}.svg`'>
                    </div>
                </div>
            </div>
        </div>
        <div class="middle">
            <p>{{this.time}}</p>
            <p class="am-pm">{{this.amPm}}</p>
        </div>
        <div class="bottom">
            <div>
                <p>{{this.week_day[this.today]}}</p>
                <p class="days-deg">{{Math.round(this.week[0].temp.max)}}&deg;C</p>
                <img class="days-result" :src='`/dist/${this.week[0].weather[0].main}.svg`'>
            </div>
            <div>
                <p>{{this.week_day[this.today + this.number[0]]}}</p>
                <p class="days-deg">{{Math.round(this.week[1].temp.max)}}&deg;C</p>
                <img class="days-result" :src='`/dist/${this.week[1].weather[0].main}.svg`'>
            </div>
            <div>
                <p>{{this.week_day[this.today + this.number[1]]}}</p>
                <p class="days-deg">{{Math.round(this.week[2].temp.max)}}&deg;C</p>
                <img class="days-result" :src='`/dist/${this.week[2].weather[0].main}.svg`' >
            </div>
            <div>
                <p>{{this.week_day[this.today + this.number[2]]}}</p>
                <p class="days-deg">{{Math.round(this.week[3].temp.max)}}&deg;C</p>
                <img class="days-result" :src='`/dist/${this.week[3].weather[0].main}.svg`'>
            </div>
            <div>
                <p>{{this.week_day[this.today + this.number[3]]}}</p>
                <p class="days-deg">{{Math.round(this.week[4].temp.max)}}&deg;C</p>
                <img class="days-result" :src='`/dist/${this.week[4].weather[0].main}.svg`'>
            </div>
            <div>
                <p>{{this.week_day[this.today + this.number[4]]}}</p>
                <p class="days-deg">{{Math.round(this.week[5].temp.max)}}&deg;C</p>
                <img class="days-result" :src='`/dist/${this.week[5].weather[0].main}.svg`'>
            </div>
            <div>
                <p>{{this.week_day[this.today + this.number[5]]}}</p>
                <p class="days-deg">{{Math.round(this.week[6].temp.max)}}&deg;C</p>
                <img class="days-result" :src='`/dist/${this.week[6].weather[0].main}.svg`'>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
        Api_key : "ae9c6534b2da2393a44f0536e1e5e1aa" ,
        url_base : "https://api.openweathermap.org/data/2.5/" ,
        search: "" ,
        location: {} ,
        weather: {} ,
        lat : [] ,
        lon: [] ,
        week : [],
        today : 0,
        number : 0 ,
        week_day: ['Sunday' , 'Monday' , 'Tuesday' , 'Wednesday' , 'Thursday' , 'Friday' , 'Saturday'],
        country : "" ,
        currentTemp : "",
        time : "" ,
        amPm: "" , 
        yes : true ,
        no : false
    }
  },
  methods:{
        fetchWeather(e){
            document.getElementById("error").style.display = "none";
            if(e.key == "Enter" || e.keyCode === 13){
                fetch(`${this.url_base}weather?q=${this.search}&units=metric&appid=${this.Api_key}`)
                .then(res =>{
                    return res.json();
                }).then(data =>{
                    return this.setR(data);
                }).then(data =>{
                     fetch(`${this.url_base}onecall?lat=${this.lat[0]}&lon=${this.lon[0]}&units=metric&exclude=hourly,minutly&appid=${this.Api_key}`)
                .then(res =>{
                    return res.json();
                }).then(data =>{
                    return this.setResults(data);
                })
                })
            }
        },
        setResults(data){
            this.weather = data;
            this.week = data.daily;
            var moment = require('moment-timezone');
            var time = moment().tz(data.timezone).format('hh:mm a');
            this.time = time.slice(0 ,5);
            var amPm = time.slice(6);
            this.amPm = amPm;
            this.currentTemp = Math.round(data.current.temp);
            if(amPm == "pm"){
                    this.yes = false ;
                    this.no = true ;
                }else{
                    this.yes = true ;
                    this.no = false
                }
            
            var d = new Date();
            this.today = d.getDay();
            if(this.today == 0){
                this.number = [1,2,3,4,5,6] ;
            }else if(this.today == 1){
                this.number = [1,2,3,4,5,-1] ;
            }else if(this.today == 2){
                this.number = [1,2,3,4,-2,-1] ;
            }else if(this.today == 3){
                this.number = [1,2,3,-3,-2,-1] ;
            }else if(this.today == 4){
                this.number = [1,2,-4,-3,-2,-1] ;
            }else if(this.today == 5){
                this.number = [1,-5,-4,-3,-2,-1] ;
            }else if(this.today == 6){
                this.number = [-6,-5,-4,-3,-2,-1] ;
            }
        },
        setR(data){
            this.location = data;
            this.country = data.sys.country;
            this.lat[0] = data.coord.lat;
            this.lon[0] = data.coord.lon;
        },
        Loading(){
            document.getElementsByClassName('loading')[0].style.display = "grid";
        },
  },
  mounted(){
    document.getElementsByClassName('loading')[0].style.display = "grid";
    let Lat;
    let Long;
    if(navigator.geolocation){
        navigator.geolocation.getCurrentPosition(pos =>{
            Lat = pos.coords.latitude;
            Long =  pos.coords.longitude;
            let Api_key = "ae9c6534b2da2393a44f0536e1e5e1aa" ;
            let url_base= "https://api.openweathermap.org/data/2.5/"; 
            fetch(`${url_base}onecall?lat=${Lat}&lon=${Long}&units=metric&exclude=hourly,minutly&appid=${Api_key}`)
            .then(res =>{
                return res.json();
            }).then(data =>{
                console.log(data);
                this.weather = data;
            this.week = data.daily;
            var moment = require('moment-timezone');
            var time = moment().tz(data.timezone).format('hh:mm a');
            this.time = time.slice(0 ,5);
            var amPm = time.slice(6);
            this.amPm = amPm;
            this.currentTemp = Math.round(data.current.temp);
            if(amPm == "pm" && 12 > this.time > 6){
                    this.yes = false ;
                    this.no = true ;
                }else{
                    this.yes = true ;
                    this.no = false
                }
            
            var d = new Date();
            this.today = d.getDay();
            if(this.today == 0){
                this.number = [1,2,3,4,5,6] ;
            }else if(this.today == 1){
                this.number = [1,2,3,4,5,-1] ;
            }else if(this.today == 2){
                this.number = [1,2,3,4,-2,-1] ;
            }else if(this.today == 3){
                this.number = [1,2,3,-3,-2,-1] ;
            }else if(this.today == 4){
                this.number = [1,2,-4,-3,-2,-1] ;
            }else if(this.today == 5){
                this.number = [1,-5,-4,-3,-2,-1] ;
            }else if(this.today == 6){
                this.number = [-6,-5,-4,-3,-2,-1] ;
            }
            this.location.name = data.timezone.slice(data.timezone.indexOf('/') + 1);
            this.country =  data.timezone.substring(0, data.timezone.indexOf('/'));
            })
        } , err => {
              document.getElementById("error").style.display = "block";
              document.getElementsByClassName('loading')[0].style.display = "none";
        });   
    };
    

}


}
</script>

<style>
    body , html{
        margin: 0 ;
        padding : 0; 
        height: 100% ;
    }
    body{
        font-family: nunito;
        overflow: hidden;
        min-height: 450px;
    }
    .loading{
        height: 20px;
        width: 60px;
        margin: auto;
        margin-top: 200px;
        display: grid;
        grid-template-columns: auto auto auto;
        display: none
    }
    .circle{
        width: 90%;
        background-color: #fff;
        border-radius: 50%;
        animation-name: fade;
        animation-duration: 1s;
        animation-iteration-count: infinite;
    }
    .circle:nth-child(1){
        animation-delay: 0s;
    }
    .circle:nth-child(2){
        animation-delay: 0.25s;
    }
    .circle:nth-child(3){
        animation-delay: 0.5s;
    }
    #error{
        display:none;
        margin: auto;
        margin-top: 200px;
        text-align: center;
        color: #fff;
        font-weight: 100;
    }
    @keyframes fade{
        from {opacity: 1;}
        to {opacity: 0;}
    }
    .am-body{
        background: linear-gradient(rgb(10, 136, 163) 90% , rgb(255 , 255 , 255)) no-repeat;
    }
    .pm-body{
        background: linear-gradient(rgb(0, 0, 0) 50% , rgb(0 , 18 , 93)) no-repeat;
    }
    .am-second{
        background: linear-gradient(rgb(255, 255, 255)  , rgb(10, 136, 163) 50%) no-repeat;
    }
    .pm-second{
        background: linear-gradient(rgb(0, 18, 93)  , rgb(0 , 0 , 0) 50%) no-repeat;
    }
    #app{
        height: 100%;
    }
    .container{
        width: 80%;
        padding: 1% 10% 1% 8.5%;
        color: #fff;
    }
    .input{
        padding-top: 1%;
        display: block;
        width: 100%;
        padding: 17px;
        font-size: 20px;
        border:  none;
        border-radius: 30px;
        background-color: rgba(255 , 255 , 255 , 0.8);
        box-shadow: 5px 5px 7px 7px rgba(0 , 0 , 0 , 0.16) ;
        color: gray;
        height:100%
    }
    .input:focus{
        outline: none !important;
        background-color: rgba(255 , 255 , 255 , 0.9);
    }
    .input:focus::placeholder{
        color : transparent;
    }
    .second{
        padding: 17px;
        box-shadow: 5px 5px 7px 7px rgba(0 , 0 , 0 , 0.16) ;
        height: 75%;
        margin: 1% 10% 1% 8.5%;
        margin-top: 2%;
        border-radius: 20px;
    }
    .top{
        display: grid;
        grid-template-columns: 33% 33% 33%;
        font-size: 3vw;
        height: 50%        
    }
    .country-container{
        width: 75%;
        margin: auto;
        text-align: center;
    }
    .country-container p{
        margin : auto ;
        display: inline-block;
    }.country{
        font-size: 2vw;
        margin-left: 0 !important;
    }
    .sun-cloud{
        text-align: center;
        height: 100%;
    }
    .sun-cloud img{
        height: 80%;
        width: 100%;
        display: block;
        margin: auto;
    }
    .result{
        width: 100%;
        height: 50%;
        margin: auto;
        margin-top: 20%;
        font-size: 3vw;
    }
    .day-deg{
        margin: auto;
    }
    .top p{
        text-align: center;
        margin-bottom:0;
    }
    .deg{
        text-align: center !important;
        display: inline-block;
        margin: 0 !important;
        margin-top: 10px !important;
    }
    .deg-container{
        width: 100%;
        margin: auto;
        display: grid;
        grid-template-columns: 50% 50%;
    }
     .deg-container img{
        width:100%;
        height:100%;
     }
     .middle{
        text-align: center;
        font-size: 3vw;
        height:10%;
        margin: auto;
        width: 15%;
        margin-top: -3%;
        margin-bottom: 1%;
        display: grid;
        grid-template-columns: 75% 25%;
     }
     .middle p {
        display: inline-block;
        font-weight: bold;
        margin : auto;
     }
     .am-pm{
        font-size: 1.5vw ;
        margin-left: -5px ;
     }
     .bottom{
        display: grid;
        grid-template-columns: 14.3% 14.3% 14.3% 14.3% 14.3% 14.3% 14.3%;
        text-align: center;
        height: 40%;
        font-size: 1.25vw;
     }
     .bottom div{
        background: rgba(255 , 255 , 255 , 0.5);
        margin: 5px;
        border-radius: 5%;
     }
     .days-deg{
        font-size: 2vw;
        margin-bottom: 10px;
     }
     .days-result{
        width: 50% ;
        height: 30%;
     }
     .tablet-mobile{
        display: none;
     }
     .img-container{
        width: 100%;
        display: inline-block;
        text-align: center;
     }
     .img-container img{
        width: 45%;
        height: 100%;
     }
     .clock-country{
        background: rgba(255 , 255 , 255 , 0.1)
     }
     .clock{
        color: #fff;
        font-size: 6vw;
        text-align: center;
     }
     .clock p{
        display: inline-block;
        margin: 0 ;
     }
     @media screen and (max-width:1030px){
        .days-deg{
            font-size: 3vw;
        }
     }
     @media screen and (max-width:850px){
        body{
            background: rgb(10, 136, 163) ;
        }
        .am-body{
            background: rgb(10, 136, 163)no-repeat;
        }
        .pm-body{
            background: rgb(0 , 6 , 32)no-repeat;
        }
        .tablet-mobile{
            display: block;
            height: 90%;
        }
        .container{
            width: 80%;
            height: 10%;
            margin: auto;
            text-align: center;
        }
        .input{
            width: 80%;
            margin: auto;
            margin-top: 3%;
            height: 30%;
            padding: 3%;
            font-size: 4vw;
        }
        .img-container{
            width: 100%;
            height: 20%;
            display: inline-block;
            text-align: center;
        }
        .img-container img{
            width: 100%;
            height: 100%;
            margin: auto
        }
        .second{
            display: none
        }
        .country-container{
            width: 100%;
            text-align: center;
            color: #fff;
            font-size: 5vw;
        }
        .country-container p{
            margin : auto ;
            display: inline-block;
        }
        .country{
            font-size: 4vw;
            margin-left: 0 !important;
        }
        .am-pm{
            font-size: 4vw ;
        }
        .clock-country{
            height:14%;
        }
        .clock{
            margin-top: -5px;
            height:50%;
            padding: 1px;
        }
        .country-container{
            margin-top: 2px;
            height:50%;
            padding: 1px;
        }
        .week{
            height: 57%;
        }
        .week div{
            height: 8%;
            margin: 1% 1%;
            background: rgba( 255 , 255 , 255 , 0.5);
            border-radius: 20px;
            padding: 2%;
            color: #fff;
            font-weight: bold;
            font-size: 4vw;
            display: grid;
            grid-template-columns: 40% 20% auto;
            margin: 0.5% 1%;
        }
        .week div p{
            text-align: center !important;
            display: block;
            margin: auto !important;
        }
        .week div img{
            width: 100%;
            height: 100%;
        }
        @media screen and (max-width:650px){
            .week div{
                margin: 1% 1%;
            }
        }
        @media screen and (max-width:600px){
            .week div{
                margin: 1% 1%;
            }
        }
        @media screen and (max-width:450px){
            .week div{
                margin: 1% 1%;
            }
        }
        
}
</style>
