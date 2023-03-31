<template>
  <form @submit.prevent="search()">
    <input type="text" v-model="searchText" /><br />
    <button>Tìm kiếm</button>
  </form>
  <div v-if="resuft.data && resuft.data.length > 0">
    <button
      :disabled="resuft.pagination.current_page == 1"
      @click="search(resuft.pagination.current_page - 1)"
    >
      Trang trước
    </button>
    <button
      :disabled="!resuft.pagination.has_next_page"
      @click="search(resuft.pagination.current_page + 1)"
    >
      Trang sau
    </button>
  </div>
  <p v-if="loading">Loading....</p>
  <!-- <img v-if="!loading" class="fix-img" :src="urlImg" alt="" /> <br />
  <p v-if="loading">Loading chờ tao....</p>
  <button @click="changeImg()">Change Img</button> -->
  <template v-if="!loading">
    <div v-for="(anime, i) in resuft.data" :key="i">
      <div class="custom">
        <img :src="anime.images.jpg.image_url" alt="" />
        <p>{{ anime.title }}</p>
        <p>{{ anime.trailer.url }}</p>
      </div>
    </div>
  </template>
</template>

<script>
import { ref } from "vue";
import axios from "axios";
export default {
  setup() {
    const searchText = ref("");
    const loading = ref(false);
    const resuft = ref([]);
    const search = async (next_page = 1) => {
      loading.value = true;
      const params = {
        q: searchText.value,
        page: next_page,
      };
      await axios
        .get("https://api.jikan.moe/v4/anime", { params: params })
        .then((res) => {
          resuft.value = res.data;
        });
      loading.value = false;
    };
    return {
      searchText,
      loading,
      search,
      resuft,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.fix-img {
  width: 500px;
  height: 300px;
}
.custom {
  width: 300px;
  height: 100px;
  margin-bottom: 5px;
  float: inherit;
  display: inline;
}
</style>
