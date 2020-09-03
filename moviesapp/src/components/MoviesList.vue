<template>
  <BContainer>
    <h3 class="list-title">IMDb Top 250 Movies</h3>
    <BRow>
      <template v-if="isExist">
        <BCol cols="3" v-for="(movie, key) in list" :key="key">
          <MovieItem
            :movie="movie"
            @mouseover.native="onMouseOver(movie.Poster)"
            @removeItem="onRemoveItem"
          />
        </BCol>
      </template>
      <template v-else>
        <div>Emty list</div>
      </template>
    </BRow>
  </BContainer>
</template>

<script>
import { mapActions } from "vuex";
import MovieItem from "./MovieItem";

export default {
  name: "MoviesList",
  props: {
    list: {
      type: Object,
      default: () => ({})
    }
  },
  components: {
    MovieItem
  },
  computed: {
    isExist() {
      return Boolean(Object.keys(this.list).length);
    }
  },
  methods: {
    ...mapActions("movies", ["removeMovie"]),
    onMouseOver(poster) {
      this.$emit("changePoster", poster);
    },
    async onRemoveItem({ id, title }) {
      const isConfirmed = await this.$bvModal.msgBoxConfirm(`Do you want to delete ${title}?`);

      if(isConfirmed) {
        this.removeMovie(id);
      }
    }
  }
};
</script>

<style scoped>
.list-title {
  font-size: 50px;
  margin-bottom: 30px;
  color: #fff;
}
</style>