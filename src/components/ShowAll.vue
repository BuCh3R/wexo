<script setup>
import { ref } from 'vue';
import axios from 'axios';

// retrieved data structure
const movieList = [
  {genreName: "Action", genreId: 28, movies: []},
  {genreName: "Comedy", genreId: 35, movies: []},
  {genreName: "Thriller", genreId: 53, movies: []},
  {genreName: "War", genreId: 10752, movies: []},
  {genreName: "Romance", genreId: 10749, movies: []},
  {genreName: "Drama", genreId: 18, movies: []},
  {genreName: "Crime", genreId: 80, movies: []},
  {genreName: "Documentary", genreId: 99, movies: []},
  {genreName: "Horror", genreId: 27, movies: []}
];

const isLoading = ref(true);

// api config
const options = {
    method: 'GET',
    headers: {
        accept: 'application/json',
        Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI0NjQwODJlMDVjODFlZDU2NTFiNmM2YzBhNmM1ZjA0NCIsIm5iZiI6MTcyOTI5ODE2Ny4yOTgwOTEsInN1YiI6IjY3MTEyODg1NmY3NzA3YWY0MGZhYWRiYSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.FfrPz_5ktxY4bFQzfbyhjmVKLar2p7fTpziaTonREug'
    }
};

// fetch data and store
async function getMovies(page){
  try {
    let response = await axios.get(`https://api.themoviedb.org/3/discover/movie?include_adult=false&include_video=false&language=en-US&page=${page}&sort_by=popularity.desc`, options)

    // push data to all matching genres movieList object if movies < 5
    function pushMovies(loopIndex, movieIndex){
      if(response.data.results[loopIndex].genre_ids.includes(movieList[movieIndex].genreId)){
        if(movieList[movieIndex].movies.length<5){
          movieList[movieIndex].movies.push(response.data.results[loopIndex])
        }
        else{
          isLoading.value = false;
        }
      }
    }
    for(let i=0; response.data.results.length; i++){

      // one for each genreId
      pushMovies(i, 0);
      pushMovies(i, 1);
      pushMovies(i, 2);
      pushMovies(i, 3);
      pushMovies(i, 4);
      pushMovies(i, 5);
      pushMovies(i, 6);
      pushMovies(i, 7);
      pushMovies(i, 8);
    }
  } catch (error) {
    console.log(error);
  }
}

// call func to request multiple api pages
let NumOfPageRequest = 5;
for(var i=1; i<NumOfPageRequest+1; i++){
  let pageNum = i.toString();
  getMovies(pageNum);
}


let imageSrc = ref('https://image.tmdb.org/t/p/w185/');

</script>

<template>
    <p v-if="isLoading">loading...</p>
    <div v-else>
      <!-- BAD CODE PRACTICE NEED REWRITE -->
      <div v-for="item in movieList" :key="item.id">
        <div v-if="item.genreId == movieList[0].genreId" class="display-genres-container">
          <h3 class="genre-titel">ACTION</h3>
          <div v-for="movie in item.movies">
            <!-- post id to url -->
            <RouterLink :to="{ path: `/MovieDetails/${movie.id}` }">
              <div>
                <span>{{ movie.title }}</span>
                <img v-bind:src=imageSrc+movie.poster_path>
              </div>
            </RouterLink>
          </div>
        </div>
        <div v-else-if="item.genreId == movieList[1].genreId" class="display-genres-container">
          <h3 class="genre-titel">COMEDY</h3>
          <div v-for="movie in item.movies">
            <RouterLink :to="{ path: `/MovieDetails/${movie.id}` }">
              <div>
                <span>{{ movie.title }}</span>
                <img v-bind:src=imageSrc+movie.poster_path>
              </div>
            </RouterLink>
          </div>
        </div>
        <div v-else-if="item.genreId == movieList[2].genreId" class="display-genres-container">
          <h3 class="genre-titel">THRILLER</h3>
          <div v-for="movie in item.movies">
            <RouterLink :to="{ path: `/MovieDetails/${movie.id}` }">
              <div>
                <span>{{ movie.title }}</span>
                <img v-bind:src=imageSrc+movie.poster_path>
              </div>
            </RouterLink>
          </div>
        </div>
        <div v-else-if="item.genreId == movieList[3].genreId" class="display-genres-container">
          <h3 class="genre-titel">WAR</h3>
          <div v-for="movie in item.movies">
            <RouterLink :to="{ path: `/MovieDetails/${movie.id}` }">
              <div>
                <span>{{ movie.title }}</span>
                <img v-bind:src=imageSrc+movie.poster_path>
              </div>
            </RouterLink>
          </div>
        </div>
        <div v-else-if="item.genreId == movieList[4].genreId" class="display-genres-container">
          <h3 class="genre-titel">ROMANCE</h3>
          <div v-for="movie in item.movies">
            <RouterLink :to="{ path: `/MovieDetails/${movie.id}` }">
              <div>
                <span>{{ movie.title }}</span>
                <img v-bind:src=imageSrc+movie.poster_path>
              </div>
            </RouterLink>
          </div>
        </div>
        <div v-else-if="item.genreId == movieList[5].genreId" class="display-genres-container">
          <h3 class="genre-titel">DRAMA</h3>
          <div v-for="movie in item.movies">
            <RouterLink :to="{ path: `/MovieDetails/${movie.id}` }">
              <div>
                <span>{{ movie.title }}</span>
                <img v-bind:src=imageSrc+movie.poster_path>
              </div>
            </RouterLink>
          </div>
        </div>
        <div v-else-if="item.genreId == movieList[6].genreId" class="display-genres-container">
          <h3 class="genre-titel">CRIME</h3>
          <div v-for="movie in item.movies">
            <RouterLink :to="{ path: `/MovieDetails/${movie.id}` }">
              <div>
                <span>{{ movie.title }}</span>
                <img v-bind:src=imageSrc+movie.poster_path>
              </div>
            </RouterLink>
          </div>
        </div>
        <div v-else-if="item.genreId == movieList[7].genreId" class="display-genres-container">
          <h3 class="genre-titel">DOCUMENTARY</h3>
          <div v-for="movie in item.movies">
            <RouterLink :to="{ path: `/MovieDetails/${movie.id}` }">
              <div>
                <span>{{ movie.title }}</span>
                <img v-bind:src=imageSrc+movie.poster_path>
              </div>
            </RouterLink>
          </div>
        </div>
        <div v-else-if="item.genreId == movieList[8].genreId" class="display-genres-container">
          <h3 class="genre-titel">HORROR</h3>
          <div v-for="movie in item.movies">
            <RouterLink :to="{ path: `/MovieDetails/${movie.id}` }">
              <div>
                <span>{{ movie.title }}</span>
                <img v-bind:src=imageSrc+movie.poster_path>
              </div>
            </RouterLink>
          </div>
        </div>
      </div>
    </div>

</template>