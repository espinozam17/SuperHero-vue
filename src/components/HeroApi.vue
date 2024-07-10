<template>
  <div>
    <nav class="navbar bg-dark">
      <div class="container">
        <a class="navbar-brand" href="#">

        </a>
      </div>
    </nav>
    <header>
      <div class="d-flex justify-content-evenly p-3">

      </div>
    </header>

    <form @submit.prevent="setHeroes">
      <div class="container ps-5 ms-5">
        <div class="container mx-5" style="width: 800px">
          <div class="container mx-5">
            <h1>Encuentra tu SuperHero</h1>
            <p>Ingresa el número del SuperHero a buscar</p>
          </div>

          <div class="mb-3 ps-2 mx-5" style="width: 120%">
            <input v-model="input" id="heroInput" type="number" class="form-control" width="40" min="1" max="732"
              step="1" />
          </div>
          <div class="mx-5 ps-2">
            <button type="submit" class="btn btn-primary">Buscar</button>
          </div>
        </div>
      </div>
    </form>
    <hr>
    <div>
      <p id="warning" v-if="heroes.length === 0">No tienes superheroes agregados a tu colección...</p>
      <p v-else :class="{ 'text-danger': heroes.length === 12 }">Tienes {{ heroes.length }}/12 superheroes agregados</p>

    </div>
    <div class="container">
      <div class="d-flex">
        <div v-for="(heroe, index) in heroes" :key="index" class="card" style="width: 18rem;">
          <button @click="eliminarHeroe(index)" class="eliminar">X</button>
          <img :src="heroe.image.url" class="card-img-top" alt="perfil">
          <div class="card-body">

            <h4 class="card-title">Nombre: {{ heroe.name }}</h4>

            <p class="card-text">ID: {{ heroe.id }}.</p>
            <a @click="showModal(heroe)"
              style="cursor: pointer;" href="#" class="btn btn-primary">Para saber más</a>
          </div>
        </div>

      </div>
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
      props: ["heroes"],
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
        // const response = await axios.get(url);
        // return response.data;
        /* Con destructuring */
      } catch (error) {
        console.log(error);
      }
    },

    async setHeroes() {


if (this.heroes.length == 12) {

  return;
}
const heroesResponse = await this.getHeroes();
const heroRepetido = this.heroes.find(heroe => heroe.id == heroesResponse.id)
if (heroRepetido) {return alert("Este héroe ya fue seleccionado");}
if (heroesResponse) {
  this.heroes.push(heroesResponse);
}
},
    eliminarHeroe(index) {
      this.heroes.splice(index, 1);
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
.eliminar {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: red;
  color: white;
  border: none;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  text-align: center;
  line-height: 24px;
  cursor: pointer;
}
</style>