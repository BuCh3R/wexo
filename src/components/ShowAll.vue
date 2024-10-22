<script setup>
import { ref, watch} from 'vue';
// import { useDatabase } from './DBConnection.vue';
import axios from 'axios';
// const { isLoading, results, movieGen } = useDatabase();

// movieGen();

const resultsGenre = ref([]);
const isLoadingGenre = ref(true);

const results = ref([]);
const isLoading = ref(true);

let newGenre = ref([]);

const availableGenres = [28, 35, 53, 10752, 10749, 18, 80, 99, 27];

const movieList = [{genreName: "Action", genreId: 28, movies: ["en", "to", "tre"]}];
// console.log(movieList);

// let newresult = ref();

const options = {
        method: 'GET',
        headers: {
            accept: 'application/json',
            Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI0NjQwODJlMDVjODFlZDU2NTFiNmM2YzBhNmM1ZjA0NCIsIm5iZiI6MTcyOTI5ODE2Ny4yOTgwOTEsInN1YiI6IjY3MTEyODg1NmY3NzA3YWY0MGZhYWRiYSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.FfrPz_5ktxY4bFQzfbyhjmVKLar2p7fTpziaTonREug'
        }
    };
    async function getGenres(){
      try {
        let response = await axios.get('https://api.themoviedb.org/3/genre/movie/list?language=en', options)
        resultsGenre.value = response.data.genres
        // MÅSKE MAP HER
        // newGenre.value = response.data.genres.flatMap(checkMovieId);
        isLoadingGenre.value = false;
      } catch (error) {
        console.log(error);
      }
    }
    getGenres();

    async function getTenMoviePages(page){
      try {
        let response = await axios.get(`https://api.themoviedb.org/3/discover/movie?include_adult=false&include_video=false&language=en-US&page=${page}&sort_by=popularity.desc`, options)
        // results.value = response.data.results;
        results.value.push(...response.data.results)
        // console.log(response.data.results);
        isLoading.value = false;
      } catch (error) {
        console.log(error);
      }
    }
    let pageNum;
    for(var i=1; i<5; i++){
      pageNum = i.toString();
      getTenMoviePages(pageNum);
    }


// function checkMovieId(element){
//   for(var i=0; i<availableGenres.length; i++){
//     if(element.id == availableGenres[i-1]){
//       return element;
//     }
//   }
// }
let imageSrc = ref('https://image.tmdb.org/t/p/w185/');
</script>

<template>
    <p v-if="isLoadingGenre">loading...</p>
    <div v-else>
      <div v-for="item in resultsGenre" :key="item.id">
          <!-- ROUTERLINK HER -->
          Genre Name: <a href=""> {{ item.name }} </a><br>
          Genre Id: {{ item.id }} <br>
          <hr>
      </div>
    </div>
    <p v-if="isLoading">loading...</p>
    <div v-else>
      <div v-for="item in results" :key="item.id">
          {{ item.title }}
          {{ item.id }}
          <img v-bind:src=imageSrc+item.poster_path>
      </div>
    </div>
</template>