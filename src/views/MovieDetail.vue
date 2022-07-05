<template>
  <div class="movie-detail">
    <h2>{{ movie.Title }}</h2>
    <p>{{ movie.Year }}</p>
    <div class="flex">
      <img :src="movie.Poster" alt="Movie Poster" class="featured-img"/>
      <p>{{ movie.Plot }}</p>  
    </div>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import { useRoute } from 'vue-router';
import { onBeforeMount } from '@vue/runtime-core';
import env from '@/env';
export default {
  setup() {
    const movie = ref({});
    const route = useRoute();

    onBeforeMount(() => {
      fetch(`http://www.omdbapi.com/?apiKey=${env.apiKey}&i=${route.params.id}&plot=full`)
        .then(response => response.json())
        .then(data => {
          movie.value = data;
        })
    })

    return {
      movie
    }
  }
}
</script>

<style lang="scss" scoped>
  .movie-detail {
    padding: 16px;

    h2 {
      color: #fff;
      font-size: 20px;
      font-weight: 600;
      margin-bottom: 16px;
    }

    p {
      color: #fff;
      font-size: 1.2em;
      margin-bottom: 20px;
    }

    .flex {
      display: flex;

      .featured-img {
      display: block;
      max-width: 200px;
      margin-bottom: 16px;
      flex: 30%;
      }

      p {
        color: #fff;
        font-size: 18px;
        line-height: 1.4;
        flex: 70%;
        margin-left: 20px;
        text-align: justify;
      }
    }

    
  }
</style>