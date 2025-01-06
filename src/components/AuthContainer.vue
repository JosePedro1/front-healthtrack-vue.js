<template>
  <div id="app">
    <div class="container" id="authContainer">
      <!-- Tela de Cadastro -->
      <div class="content first-content" v-if="isSignupMode">
        <div class="first-column">
          <h2 class="title title-primary">Bem-vindo de volta!</h2>
          <p class="description description-primary">
            Por favor, faça login com suas informações pessoais!
          </p>
          <button @click="switchToLogin" class="btn btn-primary">Entrar</button>
        </div>
        <div class="second-column">
          <template v-if="!isSignupSuccess">
            <h2 class="title title-second">Criar Conta</h2>
            <form @submit.prevent="handleSignup" class="form">
              <label class="label-input">
                <input v-model="signupForm.username" type="text" placeholder="Nome" />
              </label>
              <label class="label-input">
                <input v-model="signupForm.email" type="email" placeholder="Email" />
              </label>
              <label class="label-input">
                <input
                  v-model="signupForm.password"
                  type="password"
                  placeholder="Senha"
                />
              </label>
              <button class="btn btn-second" :disabled="!isSignupFormValid">
                Cadastrar
              </button>
            </form>
          </template>
          <template v-else>
            <div class="success-message">
              <h2 class="title title-second">Cadastro feito com sucesso!</h2>
              <button @click="switchToLogin" class="btn btn-second">Ir para Login</button>
            </div>
          </template>
        </div>
      </div>

      <!-- Tela de Login -->
      <div class="content second-content" v-if="!isSignupMode">
        <div class="first-column">
          <h2 class="title title-primary">Olá, amigo!</h2>
          <p class="description description-primary">
            Insira seus dados pessoais para criar uma conta.
          </p>
          <button @click="switchToSignup" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="second-column">
          <h2 class="title title-second">Entrar</h2>
          <form @submit.prevent="handleLogin" class="form">
            <label class="label-input">
              <input v-model="loginForm.email" type="email" placeholder="Email" required />
            </label>
            <label class="label-input">
              <input
                v-model="loginForm.password"
                type="password"
                placeholder="Senha"
                required
              />
            </label>
            <button class="btn btn-second" type="submit">Entrar</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      // Gerencia o modo atual (Login ou Cadastro)
      isSignupMode: false,
      isSignupSuccess: false,

      // Controle de navegação
      currentView: "login", // Valores possíveis: "login" ou "tasks"

      // Formulários
      loginForm: {
        email: "",
        password: "",
      },
      signupForm: {
        username: "",
        email: "",
        password: "",
      },
    };
  },
  computed: {
    // Validação do formulário de cadastro
    isSignupFormValid() {
      const { username, email, password } = this.signupForm;
      return username.trim() && email.trim() && password.trim();
    },
  },
  methods: {
    // Alterna para a tela de login
    switchToLogin() {
      this.isSignupMode = false;
      this.isSignupSuccess = false;
      this.clearSignupForm();
    },
    // Alterna para a tela de cadastro
    switchToSignup() {
      this.isSignupMode = true;
    },
    
  handleLoginSuccess() {
    console.log("Login bem-sucedido, alterando showAuth para false.");
    this.showAuth = false;
  },
    // Processa o login
    handleLogin() {
    this.$emit("loginSuccess");
  }
    },
    // Processa o cadastro
    handleSignup() {
      if (this.isSignupFormValid) {
        this.isSignupSuccess = true;
        console.log("Dados de cadastro:", this.signupForm);
      }
    },
    // Limpa o formulário de cadastro
    clearSignupForm() {
      this.signupForm = {
        username: "",
        email: "",
        password: "",
      };
    },
  };

</script>

<style scoped>
/* Mantém o CSS original */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Open Sans", sans-serif;
}
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #2b2e2f;
}
.content {
  background-color: #ffffff;
  border-radius: 15px;
  width: 960px;
  height: 50%;
  justify-content: space-between;
  align-items: center;
  position: relative;
}
.content::before {
  content: "";
  position: absolute;
  background-color: #000000;
  width: 40%;
  height: 100%;
  border-top-left-radius: 15px;
  border-bottom-left-radius: 15px;
  left: 0;
}
.title {
  font-size: 28px;
  font-weight: bold;
  text-transform: capitalize;
}
.title-primary {
  color: #fff;
}
.title-second {
  color: #000000;
}
.description {
  font-size: 14px;
  font-weight: 300;
  line-height: 30px;
}
.description-primary {
  color: #fff;
}
.btn {
  border-radius: 15px;
  text-transform: uppercase;
  color: #fff;
  font-size: 10px;
  padding: 10px 50px;
  cursor: pointer;
  font-weight: bold;
  width: 150px;
  align-self: center;
  border: none;
  margin-top: 1rem;
}
.btn-primary {
  background-color: transparent;
  border: 1px solid #fff;
  transition: background-color 0.5s;
}
.btn-second {
  background-color: #000000;
  border: 1px solid #000000;
  transition: background-color 0.5s;
}
.first-content {
  display: flex;
}
.second-content {
  display: flex;
}
.first-column {
  text-align: center;
  width: 40%;
  z-index: 10;
}
.second-column {
  width: 60%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.form {
  display: flex;
  flex-direction: column;
  width: 55%;
}
.form input {
  height: 45px;
  width: 100%;
  border: none;
  background-color: #ecf0f1;
}
.label-input {
  background-color: #ecf0f1;
  display: flex;
  align-items: center;
  margin: 8px;
}
.success-message {
  text-align: center;
  margin-top: 20px;
}
</style>
