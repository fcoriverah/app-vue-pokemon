<template>

  <!--input que nos permite realizar filtros en la lista de pokemones recibidas
  tanto para todos los pokemones o los que tenemos en favoritos -->
  <div class="input-div">
    <img class="img-input-lista" src="../assets/Vector.png" alt="" />
    <input class="input-lista" v-model="search" type="text" placeholder="Search"/>
  </div>

   <!-- La lista de "todos" los pokemones se trae si la variable listAll esta en true, lo que cambia
   al cambiar a la vista de favoritos, permitiendo mostrar u ocultar el div segun corresponda -->
  <div class="ListaPokemones" v-if="listAll">

    <!-- si la busqueda no tiene resultados, se mostrara el div con el warning
    permitiendo volver al inicio -->
    <div class="pokemon-card-empty" v-if="!filtrarBusquedaAll.length">
      <p class="titulo">Uh-oh!</p>
      <p class="descripcion">You look lost on your journey!</p>
      <router-link to="/"><button class="boton">Go back home</button></router-link>
    </div>

    <!-- div que realiza un ciclo for (funcion de vuejs) el cual creará las entradas segun la cantidad
    de pokemones nos haya vuelto la respuesta de axios -->
    <div class="div-lista-pokemon" v-for="(pokemon) in filtrarBusquedaAll" v-bind:key="pokemon.name">
      <div class="pokemon-card">

        <!-- si hacemos click en el div/tarjeta de cada pokemon, nos mostrará un modal con informacion
        sobre el pokemon -->
        <div class="divOpenModal" v-on:click="modalInfoPokemon(pokemon.name)">
          <p class="pokemon-name">{{ pokemon.name }}</p>
        </div>

        <!-- si hacemos click en favoritos, podremos agregarlo a favoritos, en este paso hacemos una funcion "ternary"
        en vue que nos permite realizar comparaciones, si el pokemon en este caso consultado esta en una lista de favoritos
        cambiará su icono correspondiente a la estrella amarilla (indicando que esta en favoritos) -->
        <img class="btnFavoritos" v-on:click="agregarFavoritos(pokemon)" 
        :src="favoritesAdded.includes(pokemon.name) == true ? require('../assets/Fav-Active.png') : require('../assets/Fav-Disabled.png')" alt=""/>
      </div>
    </div>
  </div>

  <!-- La lista de los pokemones favoritos, se trae si la variable listFavorites esta en true, lo que cambia
   al cambiar a la vista de All, permitiendo mostrar u ocultar el div segun corresponda -->
  <div class="ListaPokemones" v-if="listFavorites">

    <!-- si la busqueda no tiene resultados, se mostrara el div con el warning
    permitiendo volver al inicio -->
    <div class="pokemon-card-empty" v-if="!filtrarBusquedaFav.length">
      <p class="titulo">Uh-oh!</p>
      <p class="descripcion">You look lost on your journey!</p>
      <router-link to="/"><button class="boton">Go back home</button></router-link>
    </div>

    <!-- div que realiza un ciclo for (funcion de vuejs) el cual creará las entradas segun la cantidad
    de pokemones que esten en nuestro array de favoritos -->
    <div class="div-lista-pokemon" v-for="(pokemon) in filtrarBusquedaFav" v-bind:key="pokemon.name">
      <div class="pokemon-card">

        <!-- si hacemos click en el div/tarjeta de cada pokemon, nos mostrará un modal con informacion
        sobre el pokemon -->
        <div class="divOpenModal" v-on:click="modalInfoPokemon(pokemon.name)">
          <p class="pokemon-name">{{ pokemon.name }}</p>
        </div>

        <!-- funcion "ternary" para ver si el pokemon esta en favoritos, esta vez estamos en la lista de favoritos
        si el pokemon ya existe éste se elimina debido a que el usuario clickeo el boton de agregar a favoritos nuevamente -->
        <img class="btnFavoritos" v-on:click="agregarFavoritos(pokemon)"
        :src="favoritesAdded.includes(pokemon.name) == true ? require('../assets/Fav-Active.png') : require('../assets/Fav-Disabled.png')" alt="" />
      </div>
    </div>
  </div>

  <!-- trannsicion de vueJS que en este caso se utilizará para crear y mostrar el modal con informacion.
  se mostrara si la variable showModal es true, la cual irá variando dependiendo de cada accion, por ejemplo
  el boton de cerrar la vuelve false, lo que hace que el div no se ejecute/muestre -->
  <transition name="fade" appear>
    <div class="modal-overlay" v-if="showModal">
      <div class="dataModal">

        <div class="imgPokemonModal">
          <img class="imgBackgroundModal" src="../assets/BackgroundDesktop.png" alt="" />
          <img class="imgPokemonArtworkModal" :src="imgBackgroundModal" alt="" />
          <img class="imgCerrarModal" v-on:click="showModal = false" src="../assets/CloseModal.png" alt="" />
        </div>

        <!-- datos del pokemon seleccionado -->
        <div class="datosPokemonModal">
          <p class="nombrePokemon"><span class="tagBeforeName">Name:</span> {{ nombrePokemonModal }}</p>

          <!-- hr, para hacer lineas divisoras entre los datos del pokemon -->
          <hr/>

          <p class="pesoPokemon"><span class="tagBeforeName">Weight:</span> {{ pesoPokemonModal }}</p>

          <hr/>

          <p class="alturaPokemon"><span class="tagBeforeName">Height:</span> {{ alturaPokemonModal }}</p>

          <hr/>

          <!-- se realiza un ciclo for en el tipo de pokemon que es, ésto debido a que un pokemon puede tener
          muchos tipos, por lo que se recorren los tipos rescatados y se ven mostrando, asimismo se agregó un template
          que coloca separadores por coma entre los distintos tipos de pokemon -->
          <p class="tipoPokemon"><span class="tagBeforeName">Types:</span>
            <span v-for="(tagName, index) in tipoPokemonModal" v-bind:key="index">
                <template v-if="index > 0">, </template>
                {{ tagName.type.name }}
            </span>
          </p>
          <hr/>
        </div>

        <!-- boton compartir en modal -->
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
      // Variables que se utilizaran a lo largo del flujo
      info: [],
      favorites: [],
      favoritesAdded: [],
      isFavorite: null,
      emptyList: true,
      favoritesArrayButon: [],
      search: "",
      tagName: "",
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
    // en este caso decidi usar axios, si bien pude usar fetch, axios lo encuentro mas versátil para
    // este tipo de consultas/operaciones

    // Realizamos la consulta metodo get para traer todos los pokemones y lo guardamos en la variable/data info de vue
    axios
      .get("https://pokeapi.co/api/v2/pokemon")
      .then((response) => (this.info = response.data.results));
  },
  methods: {

    // funcion para mostrar los datos de los pokemones en el moda, destacar abrimos el modal una vez tenemos la respuesta del axios
    // de esta manera evitamos abrir el modal y tener datos vacios mientras se hace la consulta
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

    // funcion para agregar a favoritos los pokemones, recirrenis el array de pokemones y si un pokemon no existe este se agrega,
    // si el pokemon ya existe se elimina del array.
    // 
    agregarFavoritos(pokemon) {
      if (this.favorites.length > 0) {
        for (var i = 0; i < this.favorites.length; i++) {
          if (this.favorites[i].name == pokemon.name) {
            this.favorites.splice(i, 1);
            this.favoritesAdded.splice(i, 1);
            return;
          }
        }
        this.favorites.push(pokemon);
        this.favoritesAdded.push(pokemon.name);
      } else {
        this.favorites.push(pokemon);
        this.favoritesAdded.push(pokemon.name);
      }
    },

    //funcion para mostrar todos los pokemones, realiza los cambios en variables para mostrar/ocultar divs corresponientes
    mostrarTodo() {
      this.listAll = true;
      this.listFavorites = false;
    },

    //funcion para mostrar los pokemones en favoritos, realiza los cambios en variables para mostrar/ocultar divs corresponientes
    mostrarFavoritos() {
      this.listAll = false;
      this.listFavorites = true;
    },
  },
  computed: {

    // funciones para filtrar los datos mediante el input, se realiza tanto para los favoritos como para filtrar todos los pokemones
    // en este caso la busqueda se filtrara por nombre de los pokemones
    filtrarBusquedaAll() {
      return this.info.filter((pokemon) => {
        return pokemon.name.match(this.search);
      });
    },
    filtrarBusquedaFav() {
      return this.favorites.filter((pokemon) => {
        return pokemon.name.match(this.search);
      });
    },
  },
};
</script>

<style lang="less">
// Definimos el tipo de lenguaje, en este caso usare LESS ya que facilita mucho el trabajo
// aparte de dejar el código mas ordenado y legible para otras personas

// importamos la URL Lato desde google
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
  overflow: auto;
  margin-top: 40px;
  height: calc(100vh - 250px);
  .pokemon-card-empty {
    .titulo {
      font-size: 36px;
      font-weight: 700;
    }
    .descripcion {
      font-size: 20px;
      font-weight: 500;
      margin-top: -15px;
    }
    .boton {
      background-color: #f22539;
      border: none;
      color: white;
      width: 155px;
      height: 44px;
      border-radius: 60px;
      font-size: 18px;
      font-family: "Lato", sans-serif;
    }
  }
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
      .btnFavoritos {
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
