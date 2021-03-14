<template>
  <div id="app">
    <PosterBg :poster="posterBg"/>
    <MoviesList :list="moviesList" @changePoster="onChangePoster"/>
    <MoviesPagination
        :current-page="currentPage"
        :per-page="moviesPerPage"
        :total="moviesLenght"
        @pageChanged="onPageChanged"
    />
  </div>
</template>

<script>
import {mapActions, mapGetters} from 'vuex';
import MoviesList from "@/components/MoviesList";
import PosterBg from "@/components/PosterBg";
import MoviesPagination from "@/components/MoviesPagination";

export default {
  name: 'App',
  components: {
    MoviesList,
    PosterBg,
    MoviesPagination
  },
  data: () => ({
    posterBg: ""
  }),
  computed: {
    ...mapGetters("movies", ["moviesList", "currentPage", "moviesPerPage", "moviesLenght"])
  },
  methods: {
    ...mapActions("movies", ["changeCurrentPage"]),
    onChangePoster(poster) {
      this.posterBg = poster;
    },
    onPageChanged(page) {
      this.$router.push({ query: { page } });
      this.changeCurrentPage(page);
    }
  },
  created() {
    if (this.$route.query.page) {
      this.changeCurrentPage(Number(this.$route.query.page));
    }
  }
}
</script>

<style>
#app {
  position: relative;
}

</style>
