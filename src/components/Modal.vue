<script setup>
import axios from 'axios';
const props = defineProps(["id"]);
const emits = defineEmits(["toggleModal"]);
const info = await axios.get(`https://api.themoviedb.org/3/movie/${props.id}`, {
      params: {
        api_key: "0a4dbc5e3b74420548bdd09d59591acf",
        append_to_response: "videos"
      },
    })
console.log(info)
</script>

<template>
  <Teleport to="body">
    <div class="modal-outer-container" @click.self="emits('toggleModal')">
      <div class="modal-inner-container">
        <button class="close-button" @click="emits('toggleModal')">X</button>
        <img :src="`https://image.tmdb.org/t/p/w500${info.data.poster_path}`" alt="">
        <div class="text">
          <h1>{{ info.data.original_title }}</h1>
          <h3>Release Date: {{ info.data.release_date}}</h3>
          <h3>Overview: {{ info.data.overview}}</h3>
        </div>
        <iframe :src="`https://www.youtube.com/embed/${info.data.videos.results.filter((video) => video.type === 'Trailer').at(0).key}`"></iframe>
      </div>
    </div>
  </Teleport>
</template>

<style scoped>
.modal-outer-container {
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
  background: #00000099;
  z-index: 3;
}
.modal-outer-container .modal-inner-container {
  background-color: black;
  color: white;
  width: clamp(480px, 100%, 1300px);
  height: 700px;
  position: relative;
  border: solid 5px rgb(159, 92, 221);
  border-radius: 1rem;
}
.modal-outer-container .modal-inner-container .close-button {
  position: absolute;
  right: -5px;
  padding: 1rem;
  background: #1F2123;
  font-weight: bold;
  font-size: 1.25rem;
  color: white;
  border: solid 5px rgb(159, 92, 221);
  border-radius: 1rem;
}
img{
  float: left;
  margin-top: 120px;
  width: 300px;
  aspect-ratio: 2/3;
  border: solid 5px rgb(159, 92, 221);
  border-radius: 1rem;
}
iframe{
  width: 630px;
  margin-left: 175px;
  aspect-ratio: 16/9;
  border: solid 5px rgb(159, 92, 221);
  border-radius: 1rem;
}
.text{
  text-align: center;
}
</style>