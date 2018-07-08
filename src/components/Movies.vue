<template>
  <div>
    <h1>IMDB movies:</h1>
    <div class="nav">
        <button @click="movies.sort(sortByScore);">Lowest rated</button>
        <button @click="movies.sort(sortByScore).reverse()">Highest rated</button>
    </div>
    <ul>
      <li :style="{ 'backgroundColor': setupColor(movie.vote_average) }" v-for="(movie, index) in movies" :key="index">
          {{movie.title}} <span> {{movie.vote_average}}</span>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
import Color from 'color'

export default {
  data () {
    return {
      movies: [],
    }
  },
  mounted () {
    axios
      .get(
        'https://api.themoviedb.org/3/discover/movie?api_key=30da710e48fc3395f30b78008442ba77'
      )
      .then(response => {
        this.movies = response.data.results.slice(0, 15)
      })
  },
  methods: {
    sortByScore: (a, b) =>
      parseFloat(a.vote_average) - parseFloat(b.vote_average),
    isBlue: rate => rate > 8.2,
    setupColor: rate => {
      const BASE_COLOR = Color.rgb(210, 248, 255)
      const darkenMultiplier = parseFloat(
        Math.round(rate) * (0.8).toFixed(1) / 10
      )
      let rgbString = BASE_COLOR.darken(darkenMultiplier)
        .rgb()
        .string()
      return rgbString
    }
  }
}
</script>
