<template>

  <div id="app" class="container-fluid">



  <div class="d-flex justify-content-md-center mt-5">
    <div class="float-md-left"><img v-if="foto" :src="foto" width="10rem" alt /></div>

    <div class="card align-self-center border-0 bg-light ml-3" style="width: 18rem;">
      <div class="card-body">
        <h5 class="card-title">{{ title }}</h5>
        <h6 class="card-subtitle mb-2 text-muted"> {{ genre }} </h6>
        <p class="card-text">{{ plot }}</p>
        <ul class="list-group mb-3">
        <li class="list-group-item">Director: {{ director }}</li>
        <li class="list-group-item">Year: {{ year }}</li>
        </ul>
        <a v-if="imdbLink" :href="imdbLink" class="card-link" target="_blank">IMDB link</a>
        <a v-if="imdbLink" href="#" class="card-link">Another link</a>
      </div>
    </div>
  </div>

  <div class="mt-3">
  <div class="d-flex justify-content-md-center">
    <form class="form-inline" @submit.prevent=searchMovie()>
      <div class="form-group mx-sm-3 mb-2">
        <label for="movieName" class="mr-2">Movie name</label>
        <input v-model="movieQuery" type="text" class="form-control col-md-30" id="movieInput" placeholder="Three Billboards Outside Ebbing, Missouri">
      </div>
      <button type="submit" class="btn btn-primary mb-2">Submit</button>
    </form>
  </div>
  </div>
    

    <!-- <form @submit.prevent="searchMovie()">
      <div class="form-group">
        <label for="movieName">Movie name</label>
        <input
          v-model="movieQuery"
          type="text"
          class="form-control"
          id="movieInput"
          aria-describedby="movieSearchHelp"
          placeholder="Search for a movie"
        />
        <small id="movieSearchHelp" class="form-text text-muted">Search for a movie</small>
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form> -->
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      movieQuery: 'Three Billboards Outside Ebbing, Missouri',
      foto: '',
      title: '',
      plot: '',
      genre: '',
      imdbLink: '',
      director: '',
      year: '',
    };
  },
  methods: {
    searchMovie() {
      fetch("https://www.omdbapi.com/?apikey=2d1ccbe&s=" + this.movieQuery)
        .then(response => {
          return response.json();
        })
        .then(data => {
          data = data.Search[0];
          fetch("https://www.omdbapi.com/?apikey=2d1ccbe&i=" + data.imdbID)
            .then(response => {
              return response.json();
            })
            .then(data => {
              console.log(data);
              this.foto = data.Poster;
              this.title = data.Title;
              this.plot = data.Plot;
              this.genre = data.Genre;
              this.imdbLink = 'https://www.imdb.com/title/' + data.imdbID;
              this.director = data.Director;
              this.year = data.Year;
            });
        });
      this.movieQuery = "";
    }
  },
  created: function () {
    this.searchMovie();
  },
};
</script>

<style>
img {
  display: block;
  margin: auto;
  width: 20rem;
}

.app {
  background: .bg-dark;
}
</style>
