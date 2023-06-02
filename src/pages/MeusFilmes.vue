<template>
    <div class="meus-filmes">
      <h1 class="title">Meus filmes locados</h1>
      <div class="filme-card-container" v-for="filme in filmesLocados" :key="filme.id">
        <FilmeCard :filme="filme" />
      </div>
    </div>
  </template>
  
  <script>
  import { appStore } from "src/stores/appStore";
  import FilmeCard from "../components/FilmeCard.vue";
  
  export default {
    components: { FilmeCard },
    data() {
      return {
        filmesLocados: [],
      };
    },
    created() {
      this.filmesLocados = appStore.carrinho.filmes;
    },
  };
  </script>
  
  <style scoped>
  .meus-filmes {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
  }
  
  .title {
    color: #333;
    font-size: 2em;
    text-align: center;
    padding-bottom: 20px;
  }
  
  .filme-card-container {
    background-color: #f5f5f5;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.15);
    margin-bottom: 20px;
    padding: 20px;
  }
  </style>
  