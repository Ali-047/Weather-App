<template>
  <div id="app">
    <div class="searchBar">
      <div id="searchCity">
        <input
          type="text"
          v-model="cityName"
          @keyup="getInfo"
          placeholder="Please enter your city"
        />
      </div>
      <div id="searchBarBtns">
        <button @click="getInfo">
          <i class="fa-solid fa-magnifying-glass"></i>
        </button>
        <button id="clear" @click="reloadPage">Clear</button>
      </div>
    </div>
    <br />
    <div class="cityInfo" v-if="showInfo">
      <h1 ref="city"></h1>
      <h1 ref="temp"></h1>
      <h1 ref="status"></h1>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      cityName: "",
      showInfo: false,
    };
  },
  methods: {
    getInfo(event) {
      if ((event.key == "Enter" && this.cityName) || event.type == "click") {
        fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.cityName}&appid=fd94a2f995e07e9876f77f0ac204fb89`
        )
          .then((res) => res.json())
          .then((data) => {
            console.log(data);
            this.$refs.city.innerHTML = `City : ${data.name}`;
            this.$refs.temp.innerHTML = `Temp : ${Math.floor(
              data.main.temp - 273
            )}`;
            this.$refs.status.innerHTML = `Status : ${data.weather[0].main}`;
          });
        this.showInfo = true;
      }
    },
    reloadPage() {
      location.reload();
    },
  },
};
</script>

<style>
#app {
  width: 100%;
  height: 100vh;
  background-image: url("./../images/images.jpg");
  background-repeat: no-repeat;
  background-size: cover;
}
.searchBar {
  display: inline-block;
  margin-top: 30vh;
  width: 50vw;
  height: 10vh;
}
#searchCity {
  display: inline-block;
  width: 50%;
  height: 50%;
}
#searchBarBtns {
  display: inline-block;
  width: 20%;
  height: 50%;
}
.searchBar input {
  display: inline-block;
  width: 98%;
  height: 85%;
  border-radius: 8px;
}
.searchBar input:focus {
  outline: none;
}
.searchBar button {
  width: 50%;
  height: 100%;
  font-size: 90%;
  border-radius: 8px;
  border: 1px solid rgb(167, 167, 167);
}
.cityInfo {
  display: inline-block;
  width: 40vw;
  height: 28vh;
  background-color: rgb(170, 170, 170);
  border-radius: 8px;
}
@media screen and (max-width: 768px) {
  #searchBarBtns , #searchCity{
    display: block;
    width: 100%;
  }
  .cityInfo{
    width: 50%;
  }
}
</style>
