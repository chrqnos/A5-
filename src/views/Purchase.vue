<script setup>
import axios from "axios";
import { ref } from 'vue';
import SiteModal from '../components/Modal.vue';

const showModal = ref(false);
const selectedId = ref(0);

const openModal = (id) => {
  showModal.value = true;
  selectedId.value = id;
};

const closeModal = () => {
  showModal.value = false;
};

let data = (await axios.get("https://api.themoviedb.org/3/trending/movie/week", {
    params: {
        api_key: "0a4dbc5e3b74420548bdd09d59591acf"
    }
})).data.results;
console.log(data)
</script>

<template>
    <div>
        <img v-for="movie in data" @click="openModal(movie.id)" class="poster" :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="">
    </div>
    <SiteModal v-if="showModal" @toggleModal="closeModal()" :id="selectedId" />
</template>

<style scoped>
img{
  width: 330px;
  margin-top: 15px;
  margin-left: 35px;
  border: solid 5px rgb(159, 92, 221);
  border-radius: 1rem;
}

</style>