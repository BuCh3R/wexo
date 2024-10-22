<script>
import { ref } from 'vue'

export function useDatabase() {
    const str = "discover/movie?include_adult=false&include_video=false&language=en-US&page=1&sort_by=popularity.desc";
    // const isLoading = ref(true);
    // const results = ref([]);
    const options = {
        method: 'GET',
        headers: {
            accept: 'application/json',
            Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI0NjQwODJlMDVjODFlZDU2NTFiNmM2YzBhNmM1ZjA0NCIsIm5iZiI6MTcyOTI5ODE2Ny4yOTgwOTEsInN1YiI6IjY3MTEyODg1NmY3NzA3YWY0MGZhYWRiYSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.FfrPz_5ktxY4bFQzfbyhjmVKLar2p7fTpziaTonREug'
        }
    };
// get api
  const movieGen = () => {
    fetch(`https://api.themoviedb.org/3/${str}`, options)
  .then(response => response.json())
  .then(data => {
    console.log(data)
    results.value = data.results
  })
  .finally(() => {
      isLoading.value = false;
    });
  }

  return {
    isLoading,
    results,
    movieGen
  }
}
</script>
