<template>
    <div class="task-container">
      <!-- Seção de Tarefas -->
      <div class="tasks-section">
        <h1 class="main-title">Minhas Tarefas</h1>
  
        <!-- Tarefas Sem Prazo -->
        <h2 class="column-title">Tarefas Sem Prazo</h2>
        <div
          v-for="task in noDeadlineTasks"
          :key="task.id"
          class="task-card"
          v-show="!isEditing || task.id !== taskForm.id"
        >
          <p><strong>Tarefa:</strong> {{ task.title }}</p>
          <p><strong>Responsável:</strong> {{ task.responsible }}</p>
          <p><strong>Prioridade:</strong> {{ task.priority }}</p>
          <p><strong>Data de Entrega:</strong> Sem prazo</p>
          <div class="button-group">
            <button @click="editTask(task)" class="btn btn-edit">Editar</button>
            <button @click="completeTask(task)" class="btn btn-complete">Concluir</button>
          </div>
        </div>
  
        <!-- Tarefas Com Prazo -->
        <h2 class="column-title">Tarefas Com Prazo</h2>
        <div
          v-for="task in pendingTasksWithDeadline"
          :key="task.id"
          class="task-card"
          v-show="!isEditing || task.id !== taskForm.id"
        >
          <p><strong>Tarefa:</strong> {{ task.title }}</p>
          <p><strong>Responsável:</strong> {{ task.responsible }}</p>
          <p><strong>Prioridade:</strong> {{ task.priority }}</p>
          <p><strong>Data de Entrega:</strong> {{ task.deadline }}</p>
          <div class="button-group">
            <button @click="editTask(task)" class="btn btn-edit">Editar</button>
            <button @click="completeTask(task)" class="btn btn-complete">Concluir</button>
          </div>
        </div>
  
        <!-- Tarefas Concluídas -->
        <h2 class="column-title">Tarefas Concluídas</h2>
        <div
          v-for="task in completedTasks"
          :key="task.id"
          class="task-card"
          v-show="!isEditing || task.id !== taskForm.id"
        >
          <p><strong>Tarefa:</strong> {{ task.title }}</p>
          <p><strong>Responsável:</strong> {{ task.responsible }}</p>
          <p><strong>Prioridade:</strong> {{ task.priority }}</p>
          <p><strong>Data de Entrega:</strong> {{ task.deadline || "Sem prazo" }}</p>
          <div class="button-group">
            <button @click="editTask(task)" class="btn btn-edit">Editar</button>
            <button @click="deleteTask(task)" class="btn btn-delete">Excluir</button>
          </div>
        </div>
      </div>
  
      <!-- Seção de Formulário -->
      <div class="form-section">
        <h2>{{ isEditing ? "Editar Tarefa" : "Adicionar Tarefas" }}</h2>
        <form @submit.prevent="isEditing ? updateTask() : addTask()">
          <input
            v-model="taskForm.title"
            type="text"
            placeholder="Digite sua tarefa"
            required
          />
          <input
            v-model="taskForm.responsible"
            type="text"
            placeholder="Responsável pela tarefa"
            required
          />
          <input
            v-model="taskForm.priority"
            type="number"
            placeholder="Grau de Prioridade (1 a 5)"
            min="1"
            max="5"
            required
          />
          <input
            v-model="taskForm.deadline"
            type="date"
            placeholder="Data de Entrega"
          />
          <button type="submit" class="btn btn-add">
            {{ isEditing ? "Salvar Alterações" : "Adicionar Tarefa" }}
          </button>
          <button
            v-if="isEditing"
            @click="cancelEdit"
            type="button"
            class="btn btn-cancel"
          >
            Cancelar
          </button>
        </form>
        <button @click="logout" class="btn btn-logout">Sair para tela de login</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        tasks: [],
        taskForm: {
          id: null,
          title: "",
          responsible: "",
          priority: 1,
          deadline: "",
          completed: false,
        },
        isEditing: false,
      };
    },
    computed: {
      noDeadlineTasks() {
        return this.tasks.filter((task) => !task.completed && !task.deadline);
      },
      pendingTasksWithDeadline() {
        return this.tasks.filter((task) => !task.completed && task.deadline);
      },
      completedTasks() {
        return this.tasks.filter((task) => task.completed);
      },
    },
    methods: {
      addTask() {
        this.tasks.push({
          ...this.taskForm,
          id: Date.now(),
          completed: false,
        });
        this.resetForm();
      },
      editTask(task) {
        this.taskForm = { ...task };
        this.isEditing = true;
      },
      updateTask() {
        const index = this.tasks.findIndex((t) => t.id === this.taskForm.id);
        if (index !== -1) {
          // Ao salvar alterações, remover o status de concluído
          this.tasks.splice(index, 1, {
            ...this.taskForm,
            completed: false, // Resetar o status
          });
        }
        this.resetForm();
      },
      completeTask(task) {
        task.completed = true;
      },
      deleteTask(task) {
        this.tasks = this.tasks.filter((t) => t.id !== task.id);
      },
      cancelEdit() {
        this.resetForm();
      },
      resetForm() {
        this.taskForm = {
          id: null,
          title: "",
          responsible: "",
          priority: 1,
          deadline: "",
          completed: false,
        };
        this.isEditing = false;
      },
      logout() {
        this.$emit("logout");
      },
    },
  };
  </script>
  
  
  <style>
  .task-container {
    display: flex;
    gap: 20px;
    padding: 20px;
    font-family: Arial, sans-serif;
  }
  
  .tasks-section {
    flex: 2;
    padding-right: 20px;
    border-right: 1px solid #ccc;
  }
  
  .main-title {
    text-align: left;
    margin-bottom: 10px;
  }
  
  .column-title {
    margin-top: 20px;
    margin-bottom: 10px;
  }
  
  .task-card {
    background-color: #f0f0f0;
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
  }
  
  .button-group {
    margin-top: 10px;
  }
  
  .btn {
    display: inline-block;
    margin-right: 5px;
    padding: 5px 10px;
    border: none;
    cursor: pointer;
  }
  
  .btn-edit {
    background-color: #007bff;
    color: white;
  }
  
  .btn-complete {
    background-color: #28a745;
    color: white;
  }
  
  .btn-delete {
    background-color: #dc3545;
    color: white;
  }
  
  .form-section {
    flex: 1;
    padding: 20px;
    border-radius: 5px;
    background-color: #fff;
  }
  
  input {
    display: block;
    width: 100%;
    margin: 10px 0;
    padding: 8px;
  }
  
  .btn-add {
    background-color: #007bff;
    color: white;
    width: 100%;
  }
  
  .btn-logout {
    background-color: #000;
    color: white;
    width: 100%;
    margin-top: 10px;
  }
  </style>
  