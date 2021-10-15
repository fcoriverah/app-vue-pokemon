<template>
  <!-- Se carga el componente de "Loading" cuando se pase a la siguiente página -->
  <transition name="fade">
    <Loading v-if="isLoading" />
  </transition>
  <div class="body">
    <div class="main-container">
      <div class="img-logo">
        <!-- imagen de pokemon, logo -->
        <img src="../assets/Mask-Group.png" alt="" />
      </div>

      <div class="div-text">
        <p class="texto-titulo">Welcome to Pokédex</p>
        <p class="texto-descripcion">
          The digital encyclopedia created by Professor Oak is an invaluable
          tool to Trainers in the Pokémon world.
        </p>
      </div>

      <!-- Botón que llama a la funcion cuando se clickea, lo que ejecuta el componente Loading llamado al principio -->
      <div class="botones">
        <button class="boton-get-started" @click="LoadingScreen()">
          Get Started
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Loading from "../components/Loading.vue";

export default {
  name: "Home",
  components: {
    Loading,
  },
  data() {
    return {
      // La variable "isLoading" se inicia en falso, cuando es true la pantalla de carga estará activa
      isLoading: false,
    };
  },
  methods: {
    // Definimos los métodos, en este caso la funcion LoadingScreen la cual para motivos de demostración puse
    // un contador de 3 segundos, debido a que la consulta es muy rápida al servicio de pokemon y la imagen de
    // carga se ve por milisegundos
    LoadingScreen() {
      this.isLoading = true;
      setTimeout(() => {
        this.$router.push("/list");
      }, 3000);
    },
  },
  mounted() {},
};
</script>

<style lang="less">
// Definimos el tipo de lenguaje, en este caso usare LESS ya que facilita mucho el trabajo
// aparte de dejar el código mas ordenado y legible para otras personas

// importamos la URL Lato desde google
@import url("https://fonts.googleapis.com/css2?family=Lato&display=swap&.css");

.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
.fade-leave-to, .fade-leave-from {
  opacity: 1;
}
.fade-enter-active, .fade-leave-active {
  transition: all 0.4s ease;
}

body {
  background-color: #e5e5e5;
}
.main-container {
  font-family: "Lato", sans-serif;
  text-align: center;
  margin: auto;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  .img-logo {
    padding-bottom: 30px;
  }
  .div-text {
    .texto-titulo {
      font-size: 26px;
      font-weight: 700;
      margin-bottom: 10px;
    }
    .texto-descripcion {
      font-size: 18px;
      font-weight: 500;
      width: 570px;
      display: inline-block;
      padding-bottom: 60px;
    }
  }
  .botones {
    .boton-get-started {
      font-family: "Lato", sans-serif;
      font-weight: 700;
      font-size: 18px;
      border-radius: 60px;
      border: none;
      height: 44px;
      width: 131px;
      color: white;
      background-color: #f22539;
    }
  }
}

// Version MOBILE, con un mediaquery que se ejecuta desde los 615px hacia abajo
@media (max-width: 615px){
  .main-container {
    .div-text {
      .texto-descripcion {
        width: 90%;
      }
    }
  }
}
</style>
