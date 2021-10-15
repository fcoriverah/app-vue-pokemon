<template>
  <Loading v-if="isLoading" />
  <div class="body">
    <div class="main-container">
      <div class="img-logo">
        <img src="../assets/Mask-Group.png" alt="" />
      </div>
      <div class="div-text">
        <p class="texto-titulo">Welcome to Pokédex</p>
        <p class="texto-descripcion">
          The digital encyclopedia created by Professor Oak is an invaluable
          tool to Trainers in the Pokémon world.
        </p>
      </div>
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
// import VueRouter from 'vue-router';

export default {
  name: "Home",
  components: {
    Loading,
  },
  data() {
    return {
      isLoading: false,
    };
  },
  methods: {
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
@import url("https://fonts.googleapis.com/css2?family=Lato&display=swap&.css");
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
