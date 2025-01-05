<template>
  <div id="app">
    <AuthContainer v-if="showAuth" @loginSuccess="showAuth = false" />
    <TaskContainer v-else @logout="showAuth = true" />
  </div>
</template>

<script>
import AuthContainer from "./components/AuthContainer.vue";
import TaskContainer from "./components/TaskContainer.vue";

export default {
  components: {
    AuthContainer,
    TaskContainer,
  },
  data() {
    return {
      showAuth: true,
    };
  },
};
</script>

<style>
/* Reset de estilos para todos os elementos */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* Estilo geral do corpo */
body {
    font-family: 'Open Sans', sans-serif;
}

/* Container centralizado */
.container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #2b2e2f;
}

/* Títulos principais */
.title {
    font-size: 28px;
    font-weight: bold;
    text-transform: capitalize;
}

/* Botão primário com borda transparente */
.btn-primary {
    background-color: transparent;
    border: 1px solid #fff;
    transition: background-color .5s;
}

/* Botão secundário com fundo preto */
.btn-second {
    background-color: #000000;
    border: 1px solid #000000;
    transition: background-color .5s;
}

</style>
