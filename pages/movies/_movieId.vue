<template>
  <LoadingBar v-if="$fetchState.pending"/>
  <div v-else class="container single-movie">
    <NuxtLink class="button" :to="{name:'index'}">Zpět</NuxtLink>
    <div class="movie-info">
      <div class="movie-img">
          <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt=""/>
      </div>
      <div class="movie-content">
        <h1> Název: {{movie.title}}</h1>
        <p class="movie-fact tagline">
          <span>Tagline:</span>"{{movie.tagline}}"
        </p>
        <p class="movie-fact">
         <span>Vydáno:</span>
                  {{
                    new Date(movie.release_date).toLocaleString('en-us',{
                      month:'numeric',
                      day:'numeric',
                      year:'numeric',
                    })
                  }}
        </p>
        <p class="movie-fact">
          <span>Délka:</span>{{movie.runtime}} minut
        </p>
        <p class="movie-fact">
          <span>Tržby:</span>
          {{
            movie.revenue.toLocaleString('en-us', {
              style: 'currency',
              currency: 'USD',
            })
          }}
        </p>
        <p class="movie-fact"><span>Popis: </span>{{movie.overview}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"
import LoadingBar from "~/components/LoadingBar.vue"

  export default {
    name: "SingleMovie",
    components: { LoadingBar },

    head(){
      return{
        title: this.movie.title,
      }
    },

    data() {
        return {
            movie:''
        }
    },
    async fetch() {
        await this.getSingleMovie();
    },
    fetchDelay: 1000,

    methods: {
        async getSingleMovie() {
            const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieId}?api_key=8f297756c11ba17f427a61dc53484f52&language=en-US`);
            const result = await data
            this.movie = result.data
        }
    },
}
</script>

<style lang="scss" scoped>
    .single-movie {
    color: #fff;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 32px 16px;
    .button {
      align-self: flex-start;
      margin-bottom: 32px;
    }
    .movie-info {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 32px;
      color: #fff;
      @media (min-width: 800px) {
        flex-direction: row;
        align-items: flex-start;
      }
      .movie-img {
        img {
          max-height: 500px;
          width: 100%;
          @media (min-width: 800px) {
            max-height: 700px;
            width: initial;
          }
        }
      }
      .movie-content {
        h1 {
          font-size: 56px;
          font-weight: 400;
        }
        .movie-fact {
          margin-top: 12px;
          font-size: 20px;
          line-height: 1.5;
          span {
            font-weight: 600;
            text-decoration: underline;
          }
        }
        .tagline {
          font-style: italic;
          span {
            font-style: normal;
          }
        }
      }
    }
  }
</style>