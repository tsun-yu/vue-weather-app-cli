<template>
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
</template>

<script>
import RainyIcon from "./weather icon/RainyIcon.vue";
import CloudyIcon from "./weather icon/CloudyIcon";
import SunnyIcon from "./weather icon/SunnyIcon.vue";

export default {
  name: "WeatherCard",
  components: {
    RainyIcon,
    CloudyIcon,
    SunnyIcon,
  },
  props: {
    results: {
      type: Array,
    },
  },
  setup() {
    const isOnly = (arr) => {
      return arr.length === 1;
    };
    const isFirst = (i, arr) => {
      return arr.length != 1 && i === 0;
    };
    const isLast = (i, arr) => {
      return i != 0 && i === arr.length - 1;
    };

    return {
      isOnly,
      isFirst,
      isLast,
    };
  },
};
</script>

<style lang="scss">
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
.cool {
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
</style>
