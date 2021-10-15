<template>
  <div class="input-div">
    <img class="img-input-lista" src="../assets/Vector.png" alt="" />
    <input class="input-lista" v-model="search" type="text" placeholder="Search"/>
  </div>

  <div class="ListaPokemones" v-if="listAll">
    <div class="div-lista-pokemon" v-for="(pokemon, index) in filteredCustomers" v-bind:key="index">
      <div class="pokemon-card">
        <div class="divOpenModal" v-on:click="modalInfoPokemon(pokemon.name)">
          <p class="pokemon-name">{{ pokemon.name }}</p>
        </div>
        <img class="FavDisabled" v-on:click="agregarFavoritos(pokemon)" src="../assets/Fav-Disabled.png" alt=""/>
      </div>
    </div>
  </div>

  <div class="ListaPokemones" v-if="listFavorites">
    <div class="div-lista-pokemon" v-for="pokemon in favorites" v-bind:key="pokemon">
      <div class="pokemon-card">
        <div class="divOpenModal" v-on:click="modalInfoPokemon(pokemon.name)">
          <p class="pokemon-name">{{ pokemon.name }}</p>
        </div>
        <img class="FavDisabled" v-on:click="agregarFavoritos(pokemon)" src="../assets/Fav-Disabled.png" alt="" />
      </div>
    </div>
  </div>

  <transition name="fade" appear>
    <div class="modal-overlay" v-if="showModal">
      <div class="dataModal">

        <div class="imgPokemonModal">
          <img class="imgBackgroundModal" src="../assets/BackgroundDesktop.png" alt="" />
          <img class="imgPokemonArtworkModal" :src="imgBackgroundModal" alt="" />
          <img class="imgCerrarModal" v-on:click="showModal = false" src="../assets/CloseModal.png" alt="" />
        </div>

        <div class="datosPokemonModal">
          <p class="nombrePokemon"><span class="tagBeforeName">Name:</span> {{ nombrePokemonModal }}</p>

          <hr/>

          <p class="pesoPokemon"><span class="tagBeforeName">Weight:</span> {{ pesoPokemonModal }}</p>

          <hr/>

          <p class="alturaPokemon"><span class="tagBeforeName">Height:</span> {{ alturaPokemonModal }}</p>

          <hr/>

          <p class="tipoPokemon"><span class="tagBeforeName">Types:</span>
            <span v-for="(testing, index) in tipoPokemonModal" v-bind:key="index">
                <template v-if="index > 0">, </template>
                {{ testing.type.name }}
            </span>
          </p>
          <hr/>
        </div>

        <div class="divBotonCompartir">
          <button class="botonCompartir">Share to my friends</button>
        </div>

      </div>
    </div>
  </transition>
</template>

<script>
import axios from "axios";

export default {
  name: "ListaPokemones",
  data() {
    return {
      info: [],
      favorites: [],
      favoritesAdded: [],
      buttonFavAdded: true,
      search: "",
      testing: "",
      listAll: true,
      listFavorites: false,
      showModal: false,
      imgBackgroundModal: "",
      nombrePokemonModal: "",
      pesoPokemonModal: "",
      alturaPokemonModal: "",
      tipoPokemonModal: "",
    };
  },
  setup() {},
  created() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon")
      .then((response) => (this.info = response.data.results));
  },
  methods: {
    modalInfoPokemon(pokemonName) {
      axios
        .get("https://pokeapi.co/api/v2/pokemon/" + pokemonName)
        .then(
          (response) => (
            (this.imgBackgroundModal =
              response.data.sprites.other["official-artwork"].front_default),
            (this.nombrePokemonModal = response.data.name),
            (this.pesoPokemonModal = response.data.weight),
            (this.alturaPokemonModal = response.data.height),
            (this.tipoPokemonModal = response.data.types),
            (this.showModal = true)
          )
        );
    },
    agregarFavoritos(pokemon) {
      console.log(this.favorites);
      if (this.favorites.length > 0) {
        for (var i = 0; i < this.favorites.length; i++) {
          if (this.favorites[i].name == pokemon.name) {
            this.favorites.splice(i, 1);
            console.log("boton favoritos desactivado");
            return;
          }
        }
        this.favorites.push(pokemon);
        this.favoritesAdded.push(pokemon.name);
        console.log("boton favoritos activado");
      } else {
        this.favorites.push(pokemon);
        this.favoritesAdded.push(pokemon.name);
        console.log("boton favoritos activado");
      }
    },
    mostrarTodo() {
      this.listAll = true;
      this.listFavorites = false;
    },
    mostrarFavoritos() {
      this.listAll = false;
      this.listFavorites = true;
    },
  },
  computed: {
    filteredCustomers() {
      return this.info.filter((pokemon) => {
        return pokemon.name.match(this.search);
      });
    },
  },
};
</script>

<style lang="less">
@import url("https://fonts.googleapis.com/css2?family=Lato&display=swap&.css");

.modal-overlay {
  font-family: "Lato", sans-serif;
  background-color: rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  .dataModal {
    border-radius: 7px;
    background-color: white;
    margin: auto;
    position: fixed;
    top: 50%;
    left: 50%;
    width: 570px;
    height: 550px;
    transform: translate(-50%, -50%);
    .imgPokemonModal {
      height: 220px;
      display: flex;
      .imgPokemonArtworkModal {
        height: 180px;
        width: 180px;
        position: absolute;
        padding-top: 20px;
        left: 0;
        right: 0;
        margin-left: auto;
        margin-right: auto;
      }
      .imgBackgroundModal {
        width: 100%;
        height: 220px;
        border-radius: 5px 5px 0 0;
      }
      .imgCerrarModal {
        position: absolute;
        right: 0;
        margin-right: 5px;
        margin-top: 5px;
      }
    }
    .datosPokemonModal {
      padding-top: 8px;
      text-transform: capitalize;
      hr {
        width: 85%;
        border: 0.3px solid #e8e8e8;
      }
      .tagBeforeName {
        padding-left: 40px;
        padding-right: 7px;
        font-size: 18px;
        font-weight: 600;
      }
      .nombrePokemon {
        font-size: 18px;
        color: black;
        text-align: left;
      }
      .pesoPokemon {
        font-size: 18px;
        color: black;
        text-align: left;
      }
      .alturaPokemon {
        font-size: 18px;
        color: black;
        text-align: left;
      }
      .tipoPokemon {
        font-size: 18px;
        color: black;
        text-align: left;
      }
    }
    .divBotonCompartir {
      display: flex;
      padding-left: 35px;
      padding-top: 10px;
      .botonCompartir {
        font-family: "Lato", sans-serif;
        font-size: 18px;
        font-weight: 700;
        background-color: #f22539;
        color: white;
        border: none;
        height: 44px;
        width: 195px;
        border-radius: 60px;
      }
    }
  }
}
.ListaPokemones {
  display: inline-block;
  overflow-x: hidden;
  overflow-y: scroll; /* Hide vertical scrollbar */
  margin-top: 40px;
  height: calc(100vh - 250px);
  .div-lista-pokemon {
    .pokemon-card {
      display: flex;
      text-align: left;
      background-color: white;
      width: 570px;
      height: 60px;
      border-radius: 5px;
      margin-top: 20px;
      .divOpenModal {
        width: 510px;
        height: 60px;
      }
      .FavDisabled {
        height: 50px;
        position: inherit;
        margin-right: 5px;
        margin-left: auto;
        display: block;
        padding-top: 5px;
      }
      .pokemon-name {
        text-transform: capitalize;
        font-size: 22px;
        font-weight: 500;
        padding-left: 20px;
        margin-top: 16px;
      }
    }
  }
}

// ----------------- MOBILE -----------------

@media (max-width: 615px) {
  .modal-overlay {
    .dataModal {
      width: 85%;
    }
  }
  .ListaPokemones {
    width: 90%;
    .div-lista-pokemon {
      .pokemon-card {
        width: auto;
        .divOpenModal {
          width: 85%;
        }
        .FavDisabled {
          margin-right: 7px;
          padding-top: 7px;
          height: 45px;
        }
      }
    }
  }
}
</style>
