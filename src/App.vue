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
      <div
        class="weatherCard"
        v-for="(v, i) of results"
        :class="{
          'weatherCard--first': isFirst(i, results),
          'weatherCard--last': isLast(i, results),
          'weatherCard--only': isOnly(results),
        }"
        :key="i"
      >
        <div class="icon">
          <RainyIcon v-if="v.weather[0].main == 'Rain'" />
          <CloudyIcon v-else-if="v.weather[0].main == 'Drizzle'" />
          <CloudyIcon v-else-if="v.weather[0].main == 'Clouds'" />
          <SunnyIcon v-else />
        </div>
        <div class="weatherInfo__location">
          <h2>{{ v.name }}</h2>
          <!-- <p>April 26 2022</p> -->
          <p>{{ v.weather[0].main }}</p>
          <p>Feels like: {{ Math.round(v.main.feels_like) }}°</p>
        </div>
        <div class="weatherInfo__temp">
          <h1>{{ Math.round(v.main.temp) }}°C</h1>
          <div>
            <p>{{ Math.round(v.main.temp_max) }}°</p>
            <p>{{ Math.round(v.main.temp_min) }}°</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import RainyIcon from "./components/weather icon/RainyIcon.vue";
import CloudyIcon from "./components/weather icon/CloudyIcon";
import SunnyIcon from "./components/weather icon/SunnyIcon.vue";

import { ref } from "vue";
export default {
  name: "App",
  components: {
    RainyIcon,
    CloudyIcon,
    SunnyIcon,
  },
  setup() {
    let api_key = "625d92b4d8d1889d9ae4efc7172270aa";
    let base_url = "https://api.openweathermap.org/data/2.5/";
    const query = ref("");
    const results = ref([]);
    const container = ref(null);
    const isOnly = (arr) => {
      return arr.length === 1;
    };
    const isFirst = (i, arr) => {
      return arr.length != 1 && i === 0;
    };
    const isLast = (i, arr) => {
      return i != 0 && i === arr.length - 1;
    };
    const getData = async () => {
      try {
        const response = await fetch(
          `${base_url}weather?q=${query.value}&units=metric&APPID=${api_key}`
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
      isOnly,
      isFirst,
      isLast,
      getData,
    };
  },
};
</script>

<style lang="scss">
:root {
  --color-primary: #def4fe;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
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

    .weatherCard {
      width: 100%;
      height: 132px;
      border-radius: 0.25rem;
      background-color: #def4fee8;
      color: #036684;
      padding: 1rem;
      backdrop-filter: blur(3px);
      transition: 0.4s ease-in;
      margin-bottom: 0.1rem;
      display: flex;

      &:hover {
        background-color: #70bbde;
        color: #fff;
      }

      .icon {
        width: 100px;
        height: 100%;
        font-size: 2rem;
        color: #000;
      }
      .weatherInfo__location {
        display: flex;
        flex-direction: column;
        justify-content: center;
        margin: 0 1rem;
        min-width: 8rem;
      }
      .weatherInfo__temp {
        display: flex;
        align-items: center;
        margin: 0 1rem;

        h1 {
          font-size: 3rem;
          margin-right: 0.5rem;
        }

        p {
          margin: 1rem 0;
        }
      }
    }
    .weatherCard--first {
      border-radius: 2rem 2rem 0.25rem 0.25rem;
    }
    .weatherCard--last {
      border-radius: 0.25rem 0.25rem 2rem 2rem;
    }
    .weatherCard--only {
      border-radius: 2rem 2rem 2rem 2rem;
    }
  }
}
.cool {
  background-image: url("./assets/6241816.jpg");

  .weatherWrap {
    .weatherCard {
      background-color: #eff0fee8;
      color: #4c5a99;

      &:hover {
        background-color: #bec1e4;
      }
    }
  }
}

@media (max-width: 540px) {
  .container {
    .weatherWrap {
      width: 100%;

      .weatherCard {
        padding: 0.4rem;

        .weatherInfo__temp {
          flex-direction: column;
          align-items: flex-start;
          justify-content: center;

          div {
            display: flex;
          }

          p {
            margin: 0 0.2rem;
          }
        }
        .weatherInfo__location {
          margin: 0;
          min-width: 0px;
          margin-left: 0.2rem;
        }
      }
    }
  }
}
</style>
