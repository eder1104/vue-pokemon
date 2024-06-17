<template>
  <div>
    <div class="container">
      <img src="../imagenes/pokeball1.png" alt="" v-show="showPoke1" ref="poke1" class="primera">
      <div  @click="pokemon()" v-show="!showPoke1 && !searchClicked">
      <img src="../imagenes/pokeball2.png" alt="" v-show="showPoke2" ref="poke2" class="segunda">
      </div>
      <!-- quasar -->
      <div class="buscar" v-show="!showPoke2 && !searchClicked">
        <q-input v-model="indatos" debounce="900" filled
          placeholder="¡bienvenido! diviértete descubriendo nuevos pokemones!" class="buscar2"></q-input>
        <button class="search2" @click="formulario()">BUSCAR!</button>
      </div>
      <!-- quasar -->
      <div class="padre" v-show="showButtons">
        <div class="menu_sup">
          <img src="../imagenes/logo.png" alt="">
          <q-input debounce="900" filled placeholder="¡bienvenido! diviértete descubriendo nuevos pokemones!"
            class="buscar3" v-model="indatos"></q-input>
          <button class="search3" @click="formulario()">BUSCAR!</button>
        </div>
        <div class="p_datos">
          <div class="datos">
            <h3>{{ nombre }}</h3>
            <h3 class="NUMERO">{{ number }}#</h3>
              <h4 id="tipe">
                tipo <h4>{{ elemento }}</h4>
              </h4>
            <h4>altura <h4>{{ altura }} PIES</h4>
            </h4>
            <h4>peso <h4>{{ peso }} KG</h4>
            </h4>
          </div>

          <div class="caja_poke">
            <img src="../imagenes/pokeball1.png" alt="pokemon" class="img_pokemon">

              <img :src="imgPokemon" alt="" class="elpoke">

          </div>
        </div>
        <div class="estadisticas">
          <h3>ESTADISTICAS</h3>
          <div>
            <h5>vida</h5>
            <h5>{{ hp }}</h5>
          </div>
          <div>
            <h5>ataque</h5>
            <h5>{{ ataque }}</h5>
          </div>
          <div>
            <h5>defensa</h5>
            <h5>{{ defensa }}</h5>
          </div>
          <div>
            <h5>ataque especial</h5>
            <h5>{{ ataque2 }}</h5>
          </div>
          <div>
            <h5>defensa especial</h5>
            <h5>{{ defensa2 }}</h5>
          </div>
          <div>
            <h5>velocidad</h5>
            <h5>{{ speed }}</h5>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";

const showPoke1 = ref(false);
const showPoke2 = ref(true);
const searchClicked = ref(false);
const showButtons = ref(false);

function pokemon() {
  showPoke1.value = !showPoke1.value;
  showPoke2.value = !showPoke2.value;
}
let imgPokemon = ref("")
let nombre = ref("")
let number = ref("")
let indatos = ref("")
let elemento = ref("")
let altura = ref("")
let peso = ref("")
let hp = ref("")
let ataque = ref("")
let defensa = ref("")
let ataque2 = ref("")
let defensa2 = ref("")
let speed = ref("")
let info =ref("")


async function formulario() {
  if (!indatos.value) {
    alert("Por favor, ingrese el número del Pokémon.");
    return;
  }

  const numero = parseInt(indatos.value, 10);
  if (isNaN(numero) || numero <= 0 || indatos.value != numero.toString()) {
    alert("Por favor, ingrese un número entero válido.");
    return;
  }

  showPoke1.value = false;
  searchClicked.value = true;
  showButtons.value = true;
  console.log(indatos.value);
  let res = await axios.get(`https://pokeapi.co/api/v2/pokemon/${indatos.value}`);
  imgPokemon.value = res.data.sprites.other["official-artwork"].front_default;
  console.log(res.data.sprites.other["official-artwork"].front_default);
  nombre.value = res.data.species.name;
  number.value = res.data.id;
  elemento.value = res.data.types[0].type.name;
  altura.value = res.data.height;
  peso.value = res.data.weight;
  hp.value = res.data.stats[0].base_stat;
  ataque.value = res.data.stats[1].base_stat;
  defensa.value = res.data.stats[2].base_stat;
  ataque2.value = res.data.stats[3].base_stat;
  defensa2.value = res.data.stats[4].base_stat;
  speed.value = res.data.stats[5].base_stat;
  color();
}

function color() {
  if (elemento.value === "poison") {
    document.querySelector('.container').style.backgroundColor = "rgba(128, 0, 128, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(128, 0, 128, 0.7)";
  } else if (elemento.value === "fire") {
    document.querySelector('.container').style.backgroundColor = "rgba(255, 0, 0, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(255, 0, 0, 0.7)";
  } else if (elemento.value === "water") {
    document.querySelector('.container').style.backgroundColor = "rgba(35, 122, 253, 0.764)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(35, 122, 253, 0.7)";
  } else if (elemento.value === "bug") {
    document.querySelector('.container').style.backgroundColor = "rgba(0, 255, 0, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(0, 255, 0, 0.7)";
  } else if (elemento.value === "normal") {
    document.querySelector('.container').style.backgroundColor = "rgba(255, 255, 255, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(255, 255, 255, 0.7)";
  } else if (elemento.value === "electric") {
    document.querySelector('.container').style.backgroundColor = "rgba(255, 255, 0, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(255, 255, 0, 0.7)";
  } else if (elemento.value === "ground") {
    document.querySelector('.container').style.backgroundColor = "rgba(224, 168, 0, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(224, 168, 0, 0.7)";
  } else if (elemento.value === "fairy") {
    document.querySelector('.container').style.backgroundColor = "rgba(255, 192, 203, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(255, 192, 203, 0.7)";
  } else if (elemento.value === "dragon") {
    document.querySelector('.container').style.backgroundColor = "rgba(0, 0, 255, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(0, 0, 255, 0.7)";
  } else if (elemento.value === "fighting") {
    document.querySelector('.container').style.backgroundColor = "rgba(255, 0, 0, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(255, 0, 0, 0.7)";
  } else if (elemento.value === "flying") {
    document.querySelector('.container').style.backgroundColor = "rgba(0, 0, 255, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(0, 0, 255, 0.7)";
  } else if (elemento.value === "ghost") {
    document.querySelector('.container').style.backgroundColor = "rgba(255, 255, 255, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(255, 255, 255, 0.7)";
  } else if (elemento.value === "grass") {
    document.querySelector('.container').style.backgroundColor = "rgba(0, 255, 0, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(0, 255, 0, 0.7)";
  } else if (elemento.value === "ice") {
    document.querySelector('.container').style.backgroundColor = "rgba(0, 255, 255, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(0, 255, 255, 0.7)";
  } else if (elemento.value === "psychic") {
    document.querySelector('.container').style.backgroundColor = "rgba(255, 0, 255, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(255, 0, 255, 0.7)";
  } else if (elemento.value === "steel") {
    document.querySelector('.container').style.backgroundColor = "rgba(0, 0, 255, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(0, 0, 255, 0.7)";
  } else if (elemento.value === "water") {
    document.querySelector('.container').style.backgroundColor = "rgba(35, 122, 253, 0.764)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(35, 122, 253, 0.7)";
  }else if (elemento.value === "rock"){
    document.querySelector('.container').style.backgroundColor = "rgba(224, 168, 0, 0.2)";
    document.querySelector('#tipe').style.backgroundColor = "rgba(224, 168, 0, 0.7)";
  } else {
    document.querySelector('.container').style.backgroundColor = "";
  }
}




</script>


<style>
body {
  background-image: url(https://e1.pxfuel.com/desktop-wallpaper/67/73/desktop-wallpaper-retro-gaming-%E2%80%A2-nature-grass-pokemon.jpg);
  background-repeat: no-repeat;
  background-size: 100% 100vh;
  z-index: 1;
  position: relative;
}

.container {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  z-index: 2;
  position: relative;
}

.segunda {
  display: flex;
  justify-content: center;
  z-index: 1;
  cursor: pointer;
  border-radius: 30%;
}

.search {
  position: absolute;
  display: flex;
  justify-content: center;
  place-items: center;
  align-items: center;
  margin-top: 5%;
  z-index: 2;
  width: 25%;
  padding: 0.5%;
  background: white;
  border: 2px black solid;
}

.search:hover {
  cursor: pointer;
  background: linear-gradient(to right, rgb(253, 74, 74) 50%, rgb(255, 255, 255) 50%);
}

.primera {
  position: absolute;
  z-index: 1;
  display: flex;
  justify-content: center;
}

.buscar {
  position: absolute;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 40px;
  padding: 0.5%;
  width: 100%;
}

.buscar2 {
  background: linear-gradient(to right, rgb(253, 74, 74) 50%, rgb(255, 255, 255) 50%);
  font-weight: 900;
  color: black;
  width: 27%;
  text-align: center;
}

.search2 {
  background-color: #fdfdfd;
  border: 4px black solid;
  border-radius: 20px;
  color: rgb(0, 0, 0);
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 4px;
}

.search2:active {
  background-color: #ffffff;
  transform: scale(0.98);
}

.search2:hover {
  background-color: #ff0000;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
}

.search2:disabled {
  background-color: #cccccc;
  color: #666666;
  cursor: not-allowed;
}

.padre {
  display: grid;
  grid-template-rows: 33% 33% 33%;
  margin-top: -10%;
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
}

.padre .estadisticas{
  padding-top: 20vh;
}

.menu_sup {
  display: grid;
  grid-template-columns: 33% 33% 33%;
  align-items: center;
  gap: 50px;
}

.menu_sup img {
  width: 100%;

}


.buscar3 {
  background: linear-gradient(to right, rgb(253, 74, 74) 50%, rgb(255, 255, 255) 50%);
  font-weight: 900;
  color: black;
  width: 100%;
  text-align: center;
}

.search3 {
  background-color: #fdfdfd;
  border: 4px black solid;
  border-radius: 20px;
  color: rgb(0, 0, 0);
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 4px;
  width: 40%;
  height: 4vh;
}

.p_datos {
  display: grid;
  grid-template-columns: 50% 50%;
  justify-content: center;
}

.datos {
  display: flex;
  flex-direction: row;
  height: 5vh;
  font-size: 20px;
  gap: 20px;
}

#tipe{
  display: flex;
  flex-direction: column;
  place-items: center;
  align-items: center;
  text-align: center;
  font-size: 40px;
  height: 4.5vh;
  margin-top: 7%;
  border-radius: 20px;
}

.img_pokemon {
  position: absolute;
  width: 60em;
  height: 39vh;
  z-index: 1;
}

.estadisticas {
  display: flex;
  flex-direction: row;

  gap: 60px;
}

.elpoke {
  position: absolute;
  width: 15em;
  z-index: 2;
  margin-top: 4%;

}

.caja_poke {
  display: flex;
  justify-content: center;
}
</style>
<!-- <img :src="imgPokemon" alt="">
    <button @click="traer()">Traer datos</button> -->

/* let imgPokemon = ref("")

async function traer(){
let res = await axios.get("https://pokeapi.co/api/v2/pokemon/346")
imgPokemon.value= res.data.sprites.other["official-artwork"].front_default
console.log(res.data.sprites.other["official-artwork"].front_default
);
} */