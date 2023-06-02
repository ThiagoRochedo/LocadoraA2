<template>
  <q-page class="q-pa-md">
    <div v-if="naoLogado()">
      <div class="text-center q-my-md">
        <h2 class="text-h6">Olá! Seja bem vindo!! Faça seu login</h2>
      </div>
      <div class="my-styled-container q-pa-lg">
        <div v-if="!showFormCliente">
          <q-form class="q-gutter-md" @submit.prevent="handleLogin">
            <q-input outlined v-model="form.email" type="email" label="Email"></q-input>
            <q-input outlined v-model="form.senha" type="password" label="Senha"></q-input>
            <div class="flex justify-center q-mt-md">
              <q-btn color="primary" type="submit">Login</q-btn>
            </div>
          </q-form>
          <div class="text-center q-my-md">
            <p class="text-subtitle2">ou cadastre-se</p>
            <q-btn color="secondary" @click="showFormCliente = !showFormCliente">Novo Cliente</q-btn>
          </div>
        </div>
        <form-cliente
          @salvarCliente="onSalvarCliente"
          v-else
          :cliente="{
            id: 0,
            nome: '',
            email: '',
            telefone: '',
            cpf: '',
          }"
        />
      </div>
    </div>

    <div v-else>
      <div class="text-right q-mb-md">
        <q-btn color="negative" @click="handleLogout">Logout</q-btn>
      </div>
      <!-- <div class="text-center q-mb-lg">
        <router-link to="/MeusFilmes">Meus Filmes</router-link>
      </div> -->
      <div>
        <h5>Todos os filmes</h5>
      </div>
      <div v-for="filme in filmes" :key="filme.id">

        <FilmeCard
          :filme="filme"
          :logado="!naoLogado()"
          @locarFilme="onLocarFilme"
          @comprarFilme="onComprarFilme"
          @filmeLocado="onFilmeLocado"
        />
      </div>
    </div>
  </q-page>
</template>

<style>
.my-styled-container {
  max-width: 500px;
  margin: 0 auto;
  border-radius: 10px;
  background-color: #fff;
  box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.1);
}
</style>


<script>
import { useQuasar } from "quasar";
import FormCliente from "src/components/FormCliente.vue";
import Services from "src/services";
import { appStore } from "src/stores/appStore";
import { defineComponent, ref } from "vue";
import FilmeCard from "../components/FilmeCard.vue";


export default defineComponent({
  components: { FormCliente, FilmeCard },
  name: "IndexPage",
  data() {
    return {
      filmes: Array,
      showFormCliente: false,
      form: {
        email: '',
        senha: '',
      }
    };
  },
  created() {
    Services.getAllFilmes((data) => {
      this.filmes = data;
    });
  },
  methods: {
    onLocarFilme(filme) {
      appStore.carrinho.cliente = appStore.cliente;
      appStore.carrinho.filmes.push(filme);
      appStore.carrinho.data = new Date();

      Services.locarFilme(filme, (statusAtualizado) => {
        if (statusAtualizado) {
          this.filmes = this.filmes.map((f) => {
            if (f.id === filme.id) {
              f.status = false; // Indica que o filme foi locado.
            }
            return f;
          });
        }
      });
    },
    handleLogin() {
      Services.loginCliente(this.form, (loggedIn) => {
        if (loggedIn) {
          // O cliente foi autenticado com sucesso.
          appStore.cliente = {
            email: this.form.email,
            senha: this.form.senha,
          };
        } else {
          // A autenticação do cliente falhou.
          alert("Email ou senha incorretos. Por favor, tente novamente.");
        }
      });
    },
    handleLogout() {
      appStore.cliente = null; // Limpa os dados do cliente
    },
    naoLogado() {
      return appStore.cliente == null;
    },
    onSalvarCliente(cliente) {
      Services.saveCliente(cliente, (ok) => {
        if (ok) {
          this.showFormCliente = false;
          const $q = useQuasar();
          $q.notify({
            message: "Cliente salvo com sucesso!\nbem vindo " + cliente.nome,
            color: "positive",
            icon: "check",
          });
        }
      });
    },
  },
});
</script>
