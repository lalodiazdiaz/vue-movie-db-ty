<template>
  <div class="home">
    <div class="feature-card">
      <div>
        <img
          src="https://payload.cargocollective.com/1/11/367710/13568488/MOVIECLASSICSerikweb_2500_1340_c.jpg"
          alt="Poster image"
          class="featured-img"
        />
      </div>
    </div>
    <form @submit.prevent="searchMovies()" class="search-box">
      <input
        type="text"
        placeholder="What are you looking for?"
        v-model="search"
      />
      <input type="submit" value="Search" />
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="MoviePoster" />
            <div class="type">
              {{ movie.Type }}
            </div>
          </div>
          <div class="detail">
            <p class="year">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import env from "@/env";
import Swal from "sweetalert2";

export default {
  setup() {
    const search = ref("");
    const movies = ref([]);
    const url = "http://www.omdbapi.com/?";

    const searchMovies = () => {
      if (search.value != "") {
        fetch(`${url}apikey=${env.apikey}&s=${search.value}`)
          .then((response) => response.json())
          .then((data) => {
            movies.value = data.Search;
            search.value = "";
          });
      } else {
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: "You should enter a movie name in the field to do a search",
        });
      }
    };
    return { search, movies, searchMovies };
  },
};
</script>

<style lang="scss">
.feature-card {
  position: relative;
  .featured-img {
    display: block;
    width: 100%;
    height: 300px;
    object-fit: cover;

    position: relative;
    z-index: 0;
  }
  .detail {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.6);
    padding: 16px;
    z-index: 1;

    h3 {
      color: #fff;
      margin-bottom: 16px;
    }
    p {
      color: #fff;
    }
  }
}
.search-box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 16px;
  input {
    display: block;
    appearance: none;
    border: none;
    outline: none;
    background: none;

    &[type="text"] {
      width: 100%;
      color: #fff;
      background-color: #496583;
      font-size: 20px;
      padding: 10px 16px;
      border-radius: 8px;
      margin-bottom: 15px;
      transition: 0.4s;

      &::placeholder {
        color: #f3f3f3;
      }
      &:focus {
        box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
      }
    }

    &[type="submit"] {
      width: 100%;
      max-width: 300px;
      background-color: #42bb83;
      padding: 16px;
      border-radius: 8px;
      color: #fff;
      font-size: 20px;
      text-transform: uppercase;
      transition: 0.4s;

      &:active {
        background-color: #3b8070;
      }
    }
  }
}
.movies-list {
  display: flex;
  flex-wrap: wrap;
  margin: 0px 18px;

  .movie {
    max-width: 50%;
    flex: 1 1 25%;
    padding: 16px 8px;
    @media (max-width: 900px) {
      flex: 1 1 50%;
    }

    .movie-link {
      display: flex;
      flex-direction: column;
      height: 100%;

      .product-image {
        position: relative;
        display: block;
        height: 100%;

        img {
          display: block;
          width: 100%;
          height: 100%;
          object-fit: cover;
        }
        .type {
          position: absolute;
          padding: 8px 16px;
          background-color: #45b883;
          color: #fff;
          bottom: 16px;
          left: 0px;
          text-transform: capitalize;
        }
      }
      .detail {
        background-color: #496583;
        padding: 16px 8px;
        flex: 1 1 100%;
        border-radius: 0px 0px 8px 8px;

        .year {
          color: #aaa;
          font-size: 14px;
        }
        h3 {
          color: #fff;
          font-weight: 600;
          font-size: 18px;
        }
      }
    }
  }
}
</style>
