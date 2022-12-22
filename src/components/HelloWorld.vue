<template>
  <div>
    <div class="container-fluid p-0 m-0">
      <div class="row">
        <div class="col-lg-2">
          <v-card height="800" width="256" class="mx-auto">
            <v-navigation-drawer permanent class="navsize">
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title class="text-h6">
                    <h4 class="text-center name">
                      Weatheria
                      <v-icon size="30">mdi-weather-partly-lightning</v-icon>
                    </h4>
                  </v-list-item-title>
                  <v-list-item-subtitle> </v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>

              <v-divider></v-divider>

              <v-list dense nav>
                <v-list-item v-for="item in items" :key="item.title" link>
                  <v-list-item-icon>
                    <v-icon>{{ item.icon }}</v-icon>
                  </v-list-item-icon>

                  <v-list-item-content>
                    <v-list-item-title class="mx-2">{{
                      item.title
                    }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </v-navigation-drawer>
          </v-card>
        </div>
        <div class="col-lg-8">
          <input
            class="input"
            type="text"
            v-model="searchcity"
            placeholder="Enter City Name "
          />
          <button class="button" @click="getdata()">Search</button>

          <div>
            <h4 class="mt-3">Weather Details</h4>
            <div class="weather-data">
              <div>
                <h1 class="city">{{ this.city }}</h1>
                <p class="condition">{{ this.description }}</p>
                <h5 class="temp">
                  <v-icon color="#FF0000" class="mr-2" size="40"
                    >mdi-thermometer-high</v-icon
                  >{{ this.temp }}<span>&#176;</span>
                </h5>
              </div>
              <div>
                <img class="img" v-bind:src="this.imgsrc" />
              </div>
            </div>
          </div>
          <div>
            <h4 class="mt-3">Air Conditions</h4>
            <div class="weather-more-data">
              <div>
                <h1 class="condition">
                  <v-icon
                    color="#ADD8E6
"
                    class="mr-2"
                    size="50"
                    >mdi-air-humidifier</v-icon
                  >Humidity {{ this.humidity }}
                </h1>
              </div>
              <div>
                <h1 class="condition">
                  <v-icon color="#90EE90" class="mr-2" size="50"
                    >mdi-arrow-collapse</v-icon
                  >Pressure {{ this.pressure }}
                </h1>
                <!-- <img v-bind:src="this.imgsrc" /> -->
              </div>
              <div>
                <h1 class="condition">
                  <v-icon color="#FFD580" class="mr-2" size="50"
                    >mdi-eye-off</v-icon
                  >Visibility
                  {{ this.visibility }}
                </h1>
                <!-- <img v-bind:src="this.imgsrc" /> -->
              </div>
            </div>
          </div>
          <div>
            <h4 class="mt-3">Time</h4>
            <div class="weather-more-data">
              <div>
                <h1 class="condition">
                  <v-icon class="mr-2" size="50" color="#00FF00"
                    >mdi-clock-time-four-outline</v-icon
                  >
                  {{ this.time }}
                </h1>
              </div>
            </div>
          </div>
        </div>
        <div class="col-lg-2">
          <div class="list">
            <p
              class="city-name"
              v-for="(item, index) in citie"
              v-bind:key="index"
              @click="getdetails(item.city)"
            >
              {{ item.city }}
            </p>
          </div>
        </div>
      </div>
    </div>

    <!-- <button @click="getdata">Get data</button>
    <img v-bind:src="this.imgsrc" />
    <button @click="cities">Get cities</button> -->
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      time: "",
      citie: "",
      searchcity: "",
      imgsrc: "",
      city: "",
      temp: "",
      description: "",
      pressure: "",
      humidity: "",
      visibility: "",
      items: [
        { title: "Weather", icon: "mdi-weather-cloudy" },
        { title: "Cities", icon: "mdi-city-variant-outline" },
        { title: "Settings", icon: "mdi-cog-stop-outline" },
      ],
      right: null,
    };
  },
  props: {
    msg: String,
  },
  mounted() {
    fetch(
      "https://api.openweathermap.org/data/2.5/weather?q=Lahore&appid=306807ad7cfa2516d41e3318086b059d"
    )
      .then((response) => response.json())
      .then((data) => {
        this.time = new Date(data.dt * 1000 + data.timezone * 1000);
        this.city = data.name;
        this.temp = Math.round(data.main.temp_max - 273.15);

        this.description = data.weather[0].description;
        this.pressure = data.main.pressure;
        this.humidity = data.main.humidity;
        this.visibility = data.visibility;
        this.imgsrc = `https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;
      });
    fetch("https://countriesnow.space/api/v0.1/countries/population/cities")
      .then((response) => response.json())
      .then((data) => {
        console.log(data.data);
        this.citie = data.data;

        // data.data.map((val) => {});
      });
  },
  methods: {
    getdata() {
      console.log(this.searchcity);
      fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.searchcity}&appid=306807ad7cfa2516d41e3318086b059d`
      )
        .then((response) => response.json())
        .then((data) => {
          this.time = new Date(data.dt * 1000 + data.timezone * 1000);
          this.city = data.name;
          this.temp = Math.round(data.main.temp_max - 273.15);

          this.description = data.weather[0].description;
          this.pressure = data.main.pressure;
          this.humidity = data.main.humidity;
          this.visibility = data.visibility;
          this.imgsrc = `https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;
        });
    },
    getdetails(detail) {
      fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${detail}&appid=306807ad7cfa2516d41e3318086b059d`
      )
        .then((response) => response.json())
        .then((data) => {
          this.time = new Date(data.dt * 1000 + data.timezone * 1000);
          this.city = data.name;
          this.temp = Math.round(data.main.temp_max - 273.15);

          this.description = data.weather[0].description;
          this.pressure = data.main.pressure;
          this.humidity = data.main.humidity;
          this.visibility = data.visibility;
          this.imgsrc = `https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;
        });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.navsize {
  background-color: #eaecef !important;
}
.temp {
  margin-top: 20px;
  font-size: 28px;
}
.input {
  width: 400px;
  height: 20px;
  margin-top: 20px;
  border: none;
  border-bottom: 2px solid black;
}
input:focus {
  outline: none;
}
.img {
  height: 100px;
  width: 100px;
}
.city-name {
  font-size: 16px;
  color: black;
  cursor: pointer;

  margin-left: 10px;
  margin-right: 10px;
  margin-top: 5px;
}
.button {
  background-color: grey; /* Green */
  border: none;
  color: white;
  margin-left: 30px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  padding-top: 5px;
  padding-left: 10px;
  padding-right: 10px;
  padding-bottom: 5px;
}
.city-name:hover {
  background-color: black;
  color: white;
  border-radius: 10px;
}

.list {
  background-color: #eaecef !important;
  text-align: center;
  margin-top: 60px;
  height: 690px;
  margin-right: 10px;
  border-radius: 10px;
  overflow: scroll;
  overflow-x: hidden;
}
.city {
  font-size: 32px;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: bold;
}
.condition {
  font-size: 26px;
  font-family: Arial, Helvetica, sans-serif;
}
.icon {
  color: black;
  margin-left: 20px;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: bold;
}
.name {
  color: grey;
}
.weather-more-data {
  padding: 20px;
  background-color: #eaecef;
  margin-top: 20px;
  display: flex;
  justify-content: space-evenly;
  border-radius: 10px;
}
.weather-data {
  padding: 20px;
  background-color: #eaecef;
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  border-radius: 10px;
}
</style>
