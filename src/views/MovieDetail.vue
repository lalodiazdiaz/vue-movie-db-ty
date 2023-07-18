<template>
  <div class="movie-details">
    <h2>{{ movie.Title }}</h2>
    <div class="container">
      <img :src="movie.Poster" alt="Movie Poster" class="featured-img" />
      <div class="cont-info">
        <p>{{ movie.Plot }}</p>
        <p>Actors: {{ movie.Actors }}</p>
        <p>Writer: {{ movie.Writer }}</p>
        <p>Director: {{ movie.Director }}</p>
        <div class="ratings">
          <h2>Ratings</h2>
          <p v-for="rating in movie.Ratings" :key="rating">
            {{ rating.Source }} - ❇️ {{ rating.Value }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";
import { useRoute } from "vue-router";
import env from "@/env";
export default {
  setup() {
    const movie = ref({});
    const route = useRoute();

    onBeforeMount(() => {
      fetch(
        `http://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`
      )
        .then((res) => res.json())
        .then((data) => {
          movie.value = data;
        });
    });

    return { movie };
  },
};
</script>

<style lang="scss">
.movie-details {
  padding: 16px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  h2 {
    color: white;
    font-size: 28px;
    font-weight: 600;
    margin-bottom: 16px;
  }
  .container {
    display: flex;
    width: 100%;
    margin-top: 20px;
    background-color: rebeccapurple;
    @media (max-width: 900px) {
      flex-direction: column;
    }

    .featured-img {
      display: block;
      max-width: 500px;
      width: 50%;
      margin-bottom: 16px;
      margin-right: 50px;
      @media (max-width: 900px) {
        width: 100%;
      }
    }
    .cont-info {
      p {
        color: #fff;
        font-size: 18px;
        line-height: 1.4;
        margin-bottom: 10px;
      }
    }
  }
}
</style>
