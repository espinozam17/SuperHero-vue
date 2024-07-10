<template>
  <div>
    <nav class="navbar bg-dark">
      <div class="container">
        <a class="navbar-brand" href="#">SuperHero API</a>
      </div>
    </nav>
    <header>
      <div class="d-flex justify-content-evenly p-3"></div>
    </header>

    <form @submit.prevent="setHeroes">
      <div class="container ps-5 ms-5">
        <div class="container mx-5" style="width: 800px">
          <div class="container mx-5">
            <h1>Encuentra tu SuperHero</h1>
            <p>Ingresa el número del SuperHero a buscar</p>
          </div>
          <div class="mb-3 ps-2 mx-5" style="width: 100%">
            <input
              v-model="input"
              id="heroInput"
              type="number"
              class="form-control"
              min="1"
              max="732"
              step="1"
            />
          </div>
          <div class="mx-5 ps-2">
            <button type="submit" class="btn btn-primary">Buscar</button>
          </div>
        </div>
      </div>
    </form>

    <div>
      <p
        v-for="(heroe, index) in heroes"
        :key="index"
        @click="showModal(heroe)"
        style="cursor: pointer; color: blue;"
      >
        {{ heroe.name }}
      </p>
    </div>

    <!-- Modal -->
    <div v-if="show" class="modal" tabindex="-1" style="display: block;">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">{{ selectedHero.name }}</h5>
            <button type="button" class="btn-close" @click="closeModal"></button>
          </div>
          <div class="modal-body">
            <img :src="selectedHero.image.url" alt="Hero Image" style="width: 100%;">
            <p><b>CONEXIONES:</b> {{ selectedHero.connections["group-affiliation"] }}</p>
            <p><b>FECHA DE PUBLICACIÓN:</b> {{ selectedHero.biography.publisher }}</p>
            <p><b>OCUPACIÓN:</b> {{ selectedHero.work.occupation }}</p>
            <p><b>PRIMERA APARICIÓN:</b> {{ selectedHero.biography["first-appearance"] }}</p>
            <p><b>ALTURA:</b> {{ selectedHero.appearance.height.join(", ") }}</p>
            <p><b>PESO:</b> {{ selectedHero.appearance.weight.join(", ") }}</p>
            <p><b>ALIAS:</b> {{ selectedHero.biography.aliases.join(", ") }}</p>
          </div>
          <div class="py-3">
            <button type="button" class="btn btn-secondary" @click="closeModal">Cerrar</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  name: "HeroApi",
  data() {
    return {
      heroes: [],
      input: "",
      show: false,
      selectedHero: {},
    };
  },
  methods: {
    async getHeroes() {
      try {
        const url = "https://superheroapi.com/api.php/3033707663582647/" + this.input;
        const { data } = await axios.get(url);
        return data;
      } catch (error) {
        console.log(error);
      }
    },
    async setHeroes() {
      const heroesResponse = await this.getHeroes();
      console.log(heroesResponse);
      this.heroes.push(heroesResponse);
    },
    showModal(hero) {
      this.selectedHero = hero;
      this.show = true;
    },
    closeModal() {
      this.show = false;
    },
  },
};
</script>

<style>


</style>