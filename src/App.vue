<template>
  <div ref="container" class="container">
    <div class="searchWrap">
      <input
        type="text"
        name=""
        id="citySearch"
        placeholder="Search City"
        class="searchBar"
        @keyup.enter="getData"
        v-model="query"
      />
    </div>
    <div class="weatherWrap">
      <WeatherCard :results="results" />
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import WeatherCard from "./components/WeatherCard.vue";
export default {
  name: "App",
  components: {
    WeatherCard,
  },
  setup() {
    let api_key = "625d92b4d8d1889d9ae4efc7172270aa";
    let base_url = "https://api.openweathermap.org/data/2.5/";
    const query = ref("");
    const results = ref([]);
    const container = ref(null);

    const getData = async () => {
      console.log(query.value.trim().length);
      try {
        const response = await fetch(
          `${base_url}weather?q=${query.value.trim()}&units=metric&APPID=${api_key}`
        );
        if (!response.ok) {
          console.log(response.status);
          return;
        }

        const data = await response.json();
        // console.log(data);
        if (data.cod === "404") return;
        if (data.cod === "400") return;

        const isInResults = results.value.findIndex(
          (v) => v.name === data.name
        );
        if (isInResults != -1) return;
        const obj = {
          main: data.main,
          name: data.name,
          weather: data.weather,
        };
        obj.main.temp <= 10
          ? container.value.classList.add("cool")
          : container.value.classList.remove("cool");

        results.value.unshift(obj);
      } catch (err) {
        console.log(err);
      }
    };

    return {
      api_key,
      base_url,
      query,
      results,
      container,
      getData,
    };
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap");
:root {
  --color-primary: #def4fe;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Roboto", sans-serif;
}

.container {
  height: 100vh;
  overflow: auto;
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-image: url("./assets/4961757.jpg");
  background-size: cover;
  background-position: 50% 0%;
  transition: 0.5 ease-in;

  .searchWrap {
    width: 100%;
    margin-bottom: 2rem;

    .searchBar {
      display: block;
      width: 100%;
      height: 4rem;
      padding: 1rem 1.5rem;
      font-size: 1.5rem;
      border: none;
      outline: none;
      border-radius: 2rem;
      background-color: rgba(255, 255, 255, 0.3);
      backdrop-filter: blur(10px);
      transition: 0.4s ease-in;

      &:focus {
        background-color: rgba(255, 255, 255, 0.7);
        box-shadow: 0 5px 10px rgb(0 0 0 / 12%);
      }
    }
  }

  .weatherWrap {
    width: 80%;
    max-width: 1000px;
  }
}
.cool {
  background-image: url("./assets/6241816.jpg");
}

@media (max-width: 540px) {
  .container {
    .weatherWrap {
      width: 100%;
    }
  }
}
</style>
