<template>
    <div class="task-container" id="taskContainer">
      <h1 class="task-title">Minhas Tarefas</h1>
      <div id="task-list">
        <!-- Tarefas serão listadas aqui -->
      </div>
      <div class="task-content">
        <form @submit.prevent="addTask">
          <label for="taskTitle">Digite sua tarefa</label>
          <input v-model="taskTitle" type="text" id="taskTitle" class="task-input" placeholder="Digite sua tarefa">
          <label for="taskResponsible">Responsável pela tarefa</label>
          <input v-model="taskResponsible" type="text" id="taskResponsible" class="task-input" placeholder="Digite o responsável">
          <label for="taskPriority">Grau de Prioridade (1 a 5)</label>
          <input v-model="taskPriority" type="number" id="taskPriority" class="task-input" min="1" max="5">
          <label for="taskDate">Data de Entrega</label>
          <input v-model="taskDate" type="date" id="taskDate" class="task-input">
          <button class="task-btn task-btn-final-add-task">Adicionar Tarefa</button>
        </form>
        <button @click="logout" class="task-btn task-btn-logout">Sair para Tela de Login</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        taskTitle: "",
        taskResponsible: "",
        taskPriority: "",
        taskDate: "",
      };
    },
    methods: {
      addTask() {
        console.log({
          title: this.taskTitle,
          responsible: this.taskResponsible,
          priority: this.taskPriority,
          date: this.taskDate,
        });
      },
      logout() {
        this.$emit("logout");
      },
    },
  };
  </script>
  
  <style>
 /* Estilos para o container de tarefas */
.task-container {
    display: flex;
    flex-direction: column;
    padding: 20px;
    background-color: #fff;
    color: #333;
    width: 100vw;
    height: 100%;
}

/* Título das tarefas */
.task-title {
    font-size: 1.5rem;
    text-align: left;
    margin-bottom: 10px;
    border-bottom: 1px solid #000;
    padding-bottom: 10px;
}

/* Layout da área de tarefas */
.task-content {
    display: flex;
    justify-content: space-between;
    height: 100%;
}

/* Lista de tarefas */
.task-list {
    flex: 4;
    padding: 10px;
    border-right: 1px solid #000;
}

/* Estilo para cada item de tarefa */
.task-item {
    background-color: #9c9c9c;
    display: flex;
    justify-content: space-evenly;
    padding: 10px;
    margin-bottom: 10px;
}

/* Formulário de tarefas */
.task-form {
    flex: 2;
    padding: 10px;
    display: flex;
    flex-direction: column;
}

.task-input {
    width: 100%;
    padding: 8px;
    margin: 5px 0;
    border: 1px solid #ccc;
    border-radius: 5px;
}

/* Botão para adicionar tarefa */
.task-btn {
    background-color: #000000; 
    color: #ffffff;
    border: none;
    padding: 10px 20px;
    cursor: pointer;
    margin-top: 10px;
    border-radius: 5px;
    text-transform: uppercase;
    font-weight: bold;
    transition: background-color 0.3s;
}

.task-btn:hover {
    background-color: #2c2b2c; 
}

  </style>
  