<template>
  <div id="app">
    <div class="container">
      <h2 class="mb-2">Please rate the movies bellow...</h2>
      <movie-list @ratingsChanged="ratings = $event"/>

      <h2 v-if="Object.keys(ratings).length > 0" class="mb-2">Your recommendations...</h2>
      <recommendations :ratings="ratings"/>
    </div>
  </div>
</template>

<script>
  import MovieList from './components/MovieList.vue'
  import Recommendations from "@/components/Recommendations";

  export default {
    name: 'App',
    components: {
      Recommendations,
      MovieList
    },
    data: function(){
      return {
        ratings: {}
      }
    }
  }
</script>

<style>
  .movie-container{
    width:200px;
    display:inline-flex;
    margin-right:20px;
  }
</style>
