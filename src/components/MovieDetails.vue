<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { useRoute } from 'vue-router';

// get chosen movie id
const route = useRoute();
const chosenItemId = route.params.id;

const resultDetails = ref([]);
const isLoading = ref(true);


let hours = ref();
let minutes = ref();

// convert minutes to clock format
function parseMinutes(x) {
  hours.value = Math.floor(x / 60);
  minutes.value = x % 60;
  return hours.value + " h " + minutes.value + " min";
}
parseMinutes(resultDetails.runtime);

// api config
const options = {
    method: 'GET',
    headers: {
        accept: 'application/json',
        Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI0NjQwODJlMDVjODFlZDU2NTFiNmM2YzBhNmM1ZjA0NCIsIm5iZiI6MTcyOTI5ODE2Ny4yOTgwOTEsInN1YiI6IjY3MTEyODg1NmY3NzA3YWY0MGZhYWRiYSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.FfrPz_5ktxY4bFQzfbyhjmVKLar2p7fTpziaTonREug'
    }
};

// fetch and store movie details from chosen movie
async function getMovieDetails(movieId){
    try {
        let response = await axios.get(`https://api.themoviedb.org/3/movie/${movieId}?language=en-US`, options)
        resultDetails.value = response.data;
        isLoading.value = false;
    } catch (error) {
        console.log(error);
    }
}
getMovieDetails(chosenItemId);

let imageBG = ref('https://image.tmdb.org/t/p/w1280/');

</script>

<template>
    <div class="details-wrap">
      <img v-bind:src=imageBG+resultDetails.backdrop_path>
      <div class="darken-overlay">
        <div class="details-text-container">
            <h2>{{ resultDetails.title }}</h2>
            <p>{{ resultDetails.overview }}</p>
            <p><span>Ratings: {{ Math.floor(resultDetails.vote_average * 100) / 100 }}</span><span>{{ parseMinutes(resultDetails.runtime) }}</span><span>{{ resultDetails.release_date }}</span></p>
        </div>
      </div>
    </div>
</template>