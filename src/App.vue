<template>
  <div>
    <div class="principal">
      <div style="width: 100%;justify-content: space-between;display: flex;background-color: rgb(136, 52, 161);align-items: center;justify-content: space-around;">
        <div style="display: flex; justify-content: center">
          <input type="text" placeholder="¿Qué desea buscar?" v-model="nombrepokemon"
            style="border-radius: 2px; text-align: center" @input="filtrarPokemonPorNombre" />
        </div>
        <button class="inicial" @click="recargarPagina">
          <img :src="pokemon" style="background-color: transparent; height: 70px" />
        </button>
        <div style="
            display: flex;
            justify-content: space-around;
            width: 23%;
            align-items: center;
          ">
          <h1 style="color: white">POKEDEX</h1>
          <select class="form-select" aria-label="Default select example" id="tiposs" v-model="tipoSeleccionado" placeholder="tipos" @change="filtrarpokemon()" style="text-align: center;">
            <option value="0" disabled selected>Seleccione un Tipo</option>
            <option value="Todos">Todos</option>
            <option v-for="(color, tipo) in colorestipo" :key="tipo" :value="tipo">
              {{ tipo }}
            </option>
          </select>
        </div>
      </div>
      <div class="barra">
        <img :src="imagen" alt="" class="imagen" />
      </div>
      <div v-for="(pokemon, index) in pokemons" :key="index" class="card"
        v-if="activarfiltro == false && filtronombre == false">
        <button class="card-content" @click="abrirDetalle(index)">
          <div class="card-number">#{{ pokemon.numero }}</div>
          <div class="card-name">{{ pokemon.nombre }}</div>
          <div>Altura: {{ pokemon.estadistica }}</div>
          <div>Peso: {{ pokemon.peso }}kg</div>
          <img :src="pokemon.img" alt="Imagen de {{ pokemon.nombre }}" class="pokemon-image" />
          <div style="
              display: flex;
              justify-content: center;
              height: 10%;
              gap: 10%;
            ">
            <div v-for="(tipo, i) in pokemon.tipos" :key="i" :style="'background-color: ' + colorestipo[tipo]"
              class="pokemon-type">
              <p>{{ tipo }}</p>
            </div>
          </div>
        </button>
      </div>

      <div v-for="(pokemon, index) in filtropokemones" :key="index" class="card"
        v-if="activarfiltro == true && filtronombre == false">
        <button class="card-content" @click="abrirDetalle(index)">
          <div class="card-number">#{{ pokemon.numero }}</div>
          <div class="card-name">{{ pokemon.nombre }}</div>
          <div>Altura: {{ pokemon.estadistica }}</div>
          <div>Peso: {{ pokemon.peso }}kg</div>
          <img :src="pokemon.img" alt="Imagen de {{ pokemon.nombre }}" class="pokemon-image" />
          <div style="
              display: flex;
              justify-content: center;
              height: 10%;
              gap: 10%;
            ">
            <div v-for="(tipo, i) in pokemon.tipos" :key="i" :style="'background-color: ' + colorestipo[tipo]"
              class="pokemon-type">
              <p>{{ tipo }}</p>
            </div>
          </div>
        </button>
      </div>

      <div class="card" v-if="filtronombre == true">
        <button class="card-content" @click="abrirDetallebuscar()">
          <div class="card-number">#{{ pokemonBuscado.numero }}</div>
          <div class="card-name">{{ pokemonBuscado.nombre }}</div>
          <div>Altura: {{ pokemonBuscado.estadistica }}</div>
          <div>Peso: {{ pokemonBuscado.peso }}kg</div>
          <img :src="pokemonBuscado.img" alt="Imagen de {{ pokemonBuscado.nombre }}" class="pokemon-image" />
          <div style="
              display: flex;
              justify-content: center;
              height: 10%;
              gap: 10%;
            ">
            <div v-for="(tipo, i) in pokemonBuscado.tipos" :key="i" :style="'background-color: ' + colorestipo[tipo]"
              class="pokemon-type">
              <p>{{ tipo }}</p>
            </div>
          </div>
        </button>
      </div>
    </div>
    <div style="
        display: flex;
        justify-content: center;
        background-color: rgb(136 52 161);
        padding: 8px;
        margin-top: 2%;
      ">
      <button class="agregar" @click="agregarMas" style="background-color: rgb(63, 31, 85); color: rgb(255, 255, 255)">
        Agregar Más
      </button>
    </div>
    <div class="modal" v-if="mostrarModal">
      <div class="modal-content">
        <div style="
            display: flex;
            justify-content: space-between;
            align-items: center;
          ">
          <div style="font-size: 33px; display: flex; color: rgb(43 255 0)">
            #{{ detallePokemon.numero }}
          </div>
          <button @click="cerrarDetalle()" class="equis">❌</button>
        </div>
        <h2 style="margin: 2%; color: rgb(0, 128, 255); font-weight: bold">
          {{ detallePokemon.nombre }}
        </h2>
        <div style="
            display: flex;
            justify-content: center;
            gap: 10%;
            font-weight: bold;
          ">
          <div>Altura: {{ detallePokemon.estadistica }}M</div>
          <div>Peso: {{ detallePokemon.peso }}kg</div>
        </div>
        <img :src="detallePokemon.img" :alt="'Imagen de ' + detallePokemon.nombre" class="modal-image" />
        <div style="display: flex; flex-direction: column">
          <table>
            <tr>
              <td>HP</td>
              <td>
                <progress :value="detallePokemon.hp" max="140" style="margin-right: 6%"></progress>{{ detallePokemon.hp }}
              </td>
            </tr>
            <tr>
              <td>Ataque</td>
              <td>
                <progress :value="detallePokemon.ataque" max="140" style="margin-right: 6%"></progress>{{
                  detallePokemon.ataque }}
              </td>
            </tr>
            <tr>
              <td>Defensa</td>
              <td>
                <progress :value="detallePokemon.defensa" max="140" style="margin-right: 6%"></progress>{{
                  detallePokemon.defensa }}
              </td>
            </tr>
            <tr>
              <td>Ataque Especial</td>
              <td>
                <progress :value="detallePokemon.as" max="140" style="margin-right: 6%"></progress>{{ detallePokemon.as }}
              </td>
            </tr>
            <tr>
              <td>Defensa Especial</td>
              <td>
                <progress :value="detallePokemon.sd" max="140" style="margin-right: 6%"></progress>{{ detallePokemon.sd }}
              </td>
            </tr>
            <tr>
              <td>Velocidad</td>
              <td>
                <progress :value="detallePokemon.s" max="140" style="margin-right: 6%"></progress>{{ detallePokemon.s }}
              </td>
            </tr>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import imagen from "./1.jpg";
import pokemon from "./2.jpg";
import axios from "axios";
import { ref, onMounted } from "vue";
const mostrarModal = ref(false);
const detallePokemon = ref({});
const pokemons = ref([]);
const nombrePokemon = ref("");
const tipoSeleccionado = ref("Todos");
const colorestipo = {
  grass: "#4CAF50",
  poison: "#800080",
  fire: "#FF0000",
  flying: "#00BFFF",
  water: "#2196F3",
  bug: "#6B8E23",
  normal: "#808080",
  electric: "#FFFF00",
  ground: "#8B4513",
  fairy: "#FF69B4",
  ghost: "#4B0082",
  steel: "#A9A9A9",
  dark: "#2F4F4F",
  dragon: "#483D8B",
  rock: "#A0522D",
  psychic: "#FF1493",
  ice: "#00CED1",
  // fi
};

let ini = 1;
let final = 50;
let numeroDePokemonCargados = pokemons.value.length;
async function obtenerurlpokemon() {
  for (ini; ini <= final; ini++) {
    const response = await axios.get(
      `https://pokeapi.co/api/v2/pokemon/${ini}`
    );
    const r = response.data;
    pokemons.value.push({
      numero: r.id,
      nombre: r.name.toUpperCase(),
      estadistica: r.height,
      peso: r.weight,
      hp: Math.min(Math.max(r.stats[0].base_stat, 0), 100),
      ataque: r.stats[1].base_stat,
      defensa: r.stats[2].base_stat,
      as: r.stats[2].base_stat,
      sd: r.stats[4].base_stat,
      s: r.stats[5].base_stat,
      img: r.sprites.other["official-artwork"].front_default,
      mostrarDetalle: false,
      tipos: r.types.map((e) => e.type.name.toLowerCase()),
    });
  }
}
async function obtenerMasPokemon() {
  const numeroMaximoPokemon = 100;
  const cantidadARecargar = 50;
  const indiceInicio = numeroDePokemonCargados + 1;
  const indiceFin = indiceInicio + cantidadARecargar - 1;
  if (indiceInicio > numeroMaximoPokemon) {
    alert("¡Ya has cargado todos los Pokémon disponibles!");
    return;
  }
}
function agregarMas() {
  final += 50;
  obtenerurlpokemon();
}
const busquedaPokemon = ref("");
const filtropokemones = ref([]);
const activarfiltro = ref(false);

function filtrarpokemon() {
  if (tipoSeleccionado.value == "Todos") {
    activarfiltro.value = false;
    return;
  }

  filtropokemones.value = [];
  pokemons.value.forEach((pokemon) => {
    if (pokemon.tipos.includes(tipoSeleccionado.value)) {
      filtropokemones.value.push(pokemon);
    }
  });

  activarfiltro.value = true;
}

const pokemonBuscado = ref({});
const nombrepokemon = ref("");
const filtronombre = ref(false);

function filtrarPokemonPorNombre() {
  const nombre = nombrepokemon.value.toUpperCase();
  filtronombre.value = false;
  activarfiltro.value = false;

  if (!nombre) {
    return; // No hagas nada si el campo de búsqueda está vacío
  }

  const resultado = pokemons.value.find((pokemon) => pokemon.nombre === nombre);

  if (resultado) {
    // Si se encontró un Pokémon, muestra ese Pokémon en lugar de la lista
    pokemonBuscado.value = resultado;
    filtronombre.value = true;
  }
}
function abrirDetalle(index) {
  if(activarfiltro.value==true){
    detallePokemon.value = filtropokemones.value[index]
  }else  detallePokemon.value = pokemons.value[index];

  mostrarModal.value = true;
}
function cerrarDetalle() {
  mostrarModal.value = false;
}
function recargarPagina() {
  location.reload();
}

function abrirDetallebuscar(){
  detallePokemon.value = pokemonBuscado.value
  mostrarModal.value = true
}
onMounted(() => {
  obtenerurlpokemon();
});
</script>

<style scoped>
.principal {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 20px;
  height: 110%;
}

.principal > div:first-child {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 999;
}

.card {
  width: 250px;
  background-color: #f0f0f0;
  border-radius: 10px;
  padding: 8px;
  text-align: center;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
  font-weight: bold;
}

.pokemon-type {
  border-radius: 5px;
  width: 30%;
  height: 30px;
  font-size: 12px;
  text-align: center;
  color: white;
  font-weight: bold;
}

.card-number {
  font-size: 40px;
  color: #39e7c4;
}

.card-name {
  font-size: 16px;
  margin-top: 5px;
  margin-bottom: 8px;
}

.inicial {
  background-color: rgb(136, 52, 161);
  display: flex;
  justify-content: flex-end;
  width: 288px;
}

button {
  background-color: #9a1f8f;
  color: white;
  border: none;
  padding: 5px 15px;
  cursor: pointer;
}

.pokemon-data {
  font-size: 14px;
}

.pokemon-image {
  max-width: 100%;
  display: block;
  margin: 0 auto;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

.modal-content {
  background-color: rgb(255, 255, 255);
  padding: 12px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
  text-align: center;
}

.modal button {
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  display: flex;
}

.modal-image {
  width: 420px;
  height: auto;
  margin-top: 10px;
}

.equis {
  margin: 0%;
  background-color: transparent;
  font-size: 20px;
}

.barra {
  height: 47vh; 
  width: 90%;
  display: flex;
  justify-content: center;
  align-items: center;
}


.imagen {
  width: 45%;
  margin-bottom: -95px;
}

.agregar {
  display: flex;
}
</style>