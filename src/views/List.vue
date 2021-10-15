<template>
  <div class="list-container">

    <!-- cargamos el componente ListaPokemones el cual nos trae todos los pokemones, de esta manera
    el código queda mas limpio y con menos lineas -->
    <ListaPokemones ref="listaPokemones"></ListaPokemones>

    <div class="bottom-nav">
      <div class="bottom-nav-buttons">

        <!-- botones que al hacer click en ellos (o en la clase que envuelve al boton)
        se ejecuta la funcion para mostrar todos los pokemones (en este caso boton All) -->
        <div class="all-button-div" v-on:click="$refs.listaPokemones.mostrarTodo()">
          <img class="button-list-icon" src="../assets/List-icon.png" alt="" />
          <button class="all-button" autofocus>
            <span class="all-button-text">All</span>
          </button>
        </div>

        <!-- botones que al hacer click en ellos (o en la clase que envuelve al boton)
        se ejecuta la funcion para mostrar los pokemones en favoritos (en este caso boton Fav) -->
        <div class="fav-button-div" v-on:click="$refs.listaPokemones.mostrarFavoritos()">
          <img class="button-list-fav" src="../assets/Fav-icon.png" alt="" />
          <button class="fav-button">
            <span class="fav-button-text">Favorites</span>
          </button>
        </div>
        
      </div>
    </div>
  </div>
</template>

<script>
// importamos el componente para traer la lista de pokemones
import ListaPokemones from "../components/ListaPokemones.vue";

export default {
  name: "ListaPokemonesComp",
  components: {
    ListaPokemones,
  }
};
</script>


<style lang="less">
// Definimos el tipo de lenguaje, en este caso usare LESS ya que facilita mucho el trabajo
// aparte de dejar el código mas ordenado y legible para otras personas

// importamos la URL Lato desde google
@import url("https://fonts.googleapis.com/css2?family=Montserrat&display=swap&.css");
@import url("https://fonts.googleapis.com/css2?family=Lato&display=swap&.css");
body {
  background-color: #e5e5e5;
  margin: 0;
}
.btnselected {
  background-color: black;
  width: 200px;
  height: 200px;
}
.btninactive {
  width: 200px;
  height: 200px;
  background-color: blue;
}
.list-container {
  margin-top: 40px;
  font-family: "Lato", sans-serif;
  .img-input-lista {
    position: absolute;
    padding-top: 17px;
    padding-left: 15px;
    height: 18px;
    width: 18px;
  }
  .input-div {
    input::placeholder {
      color: #bfbfbf;
      opacity: 1; /* Firefox */
      font-weight: 550;
    }
    :-ms-input-placeholder {
      /* Internet Explorer 10-11 */
      color: #bfbfbf;
      font-weight: 550;
    }
    input::-ms-input-placeholder {
      /* Microsoft Edge */
      color: #bfbfbf;
      font-weight: 550;
    }
    input[type="text"] {
      font-family: "Montserrat", sans-serif;
      font-weight: 550;
      font-size: 16px;
    }
    .input-lista {
      padding-left: 45px;
      border-radius: 5px;
      font-size: 16px;
      background-color: white;
      border: none;
      height: 50px;
      width: 535px;
    }
  }
  .bottom-nav {
    filter: drop-shadow(0 -5px 4px rgba(0, 0, 0, 0.05));
    margin: 0 !important;
    background-color: white;
    height: 80px;
    bottom: 0;
    position: absolute;
    width: 100%;
    .bottom-nav-buttons {
      display: flex;
      margin: auto;
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      .all-button-div {
        .button-list-icon {
          margin-top: 12px;
          margin-left: 110px;
          position: absolute;
        }
        .all-button {
          font-family: "Lato", sans-serif;
          font-size: 18px;
          background-color: #bfbfbf;
          color: white;
          border: none;
          border-radius: 60px;
          height: 44px;
          width: 275px;
          margin-right: 20px;
          .all-button-text {
            padding-left: 30px;
          }
        }
        .all-button:focus {
          background-color: #F22539;
        }
        .all-button:hover {
          background-color: #C00E20;
        }
      }
      .fav-button-div {
        .button-list-fav {
          margin-top: 12px;
          margin-left: 85px;
          position: absolute;
        }
        .fav-button {
          font-family: "Lato", sans-serif;
          font-size: 18px;
          background-color: #bfbfbf;
          color: white;
          border: none;
          border-radius: 60px;
          height: 44px;
          width: 275px;
          margin-right: 20px;
          .fav-button-text {
            padding-left: 35px;
          }
        }
        .fav-button:focus {
          background-color: #F22539;
        }
        .fav-button:hover {
          background-color: #C00E20;
        }
      }
    }
  }
}

// ----------------- MOBILE -----------------

@media (max-width: 615px) {
  .list-container {
    .input-div {
      .input-lista {
        width: 80%;
      }
    }
    .bottom-nav {
      .bottom-nav-buttons {
        .all-button-div {
          .button-list-icon {
            margin-top: 11px;
            margin-left: 49px;
          }
          .all-button {
            width: 150px;
          }
        }
        .fav-button-div {
          .button-list-fav {
            margin-top: 10px;
            margin-left: 26px;
          }
          .fav-button {
            width: 150px;
            margin-right: 0;
          }
        }
      }
    }
  }
}
</style>