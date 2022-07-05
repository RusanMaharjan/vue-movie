<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img src="https://w7.pngwing.com/pngs/833/739/png-transparent-naruto-sasuke-uchiha-naruto-poster-computer-wallpaper-sasuke-uchiha.png" alt="Naruto" class="featured-img">
        <div class="detail">
          <h3>Naruto</h3>
          <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Praesentium illo officia necessitatibus recusandae ipsam, corrupti vel unde ea tempore repellendus!</p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="searchMovies()" class="search-box">
      <input type="text" v-model="search" placeholder="What are you looking for?">
      <input type="submit" value="Search">
    </form>

    <div class="movie-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster" />
            <div class="type">
              {{ movie.Type }}
            </div>
          </div>

          <div class="detail">
            <p class="y">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import env from '@/env';
export default {
  setup () {
    const search = ref("");
    const movies = ref([]);

    const searchMovies = () => {
      if(search.value != "") {
        fetch(`http://www.omdbapi.com/?apiKey=${env.apiKey}&s=${search.value}`)
          .then(response => response.json())
          .then(data => {
            movies.value = data.Search;
            search.value = "";
            console.log(movies.value);
          })
      }
    }

    return {
      search, movies, searchMovies
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
  .feature-card {
    position: relative;

    .featured-img {
      display: block;
      width: 100%;
      height: 80vh;
      object-fit: cover;

      position: relative;
      z-index: 0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.8);
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
        width: 60%;
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

        &::focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 200px;
        background-color: #42B883;
        padding: 16px;
        border-radius: 8px;
        color: #fff;
        font-size: 20px;
        text-transform: uppercase;
        transition: 0.4s;

        &:active {
          background-color: #3B8070;
        }
      }
    }
  }

  .movie-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie {
      max-width: 50%;
      flex: 25%;
      padding: 16px 8px;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }

          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: #42B883;
            color: #fff;
            bottom: 16px;
            left: 0px;
            text-transform: capitalize;
          }
        }

        .detail {
          background-color: #496983;
          padding: 16px 8px;
          flex: 25%;
          border-radius: 0px 0px 8px 8px;

          .y {
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
}
</style>