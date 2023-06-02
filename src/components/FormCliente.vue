<template>
  <q-card class="q-ma-md q-pa-md shadow-10">
    <h5>Preencha os campos abaixo:</h5>
  
    <q-form @submit="salvar" @reset="reset" class="q-gutter-md">
      <q-input outlined v-model="clienteData.nome" label="Nome" :rules="[val => !!val || 'Campo nome é obrigatório']"></q-input>
      <q-input
        outlined
        v-model="clienteData.email"
        label="E-mail"
        :rules="[
          val => !!val || 'Campo e-mail é obrigatório',
          val => (val && this.emailRegex.test(val)) || 'Por favor, insira um e-mail válido'
        ]"
      ></q-input>
      <q-input
        outlined
        v-model="clienteData.senha"
        label="Senha"
        :rules="[
          val => !!val || 'Campo senha é obrigatório',
        ]"
      ></q-input>
      <q-input
        outlined
        v-model="clienteData.telefone"
        mask="(##)#####-####"
        fill-mask="_"
        label="Telefone"
        :rules="[val => !!val || 'Campo telefone é obrigatório']"
      ></q-input>
      <q-input
        outlined
        v-model="clienteData.cpf"
        label="CPF"
        :rules="[
          val => !!val || 'Campo CPF é obrigatório',
          val => (val && this.cpfRegex.test(val)) || 'Por favor, insira um CPF válido'
        ]"
      ></q-input>
      

      <div>
        <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
        <q-btn type="submit" color="primary" label="Salvar" class="q-mt-md"></q-btn>
      </div>
    </q-form>
  </q-card>
</template>

<script>
import api from "src/services/api";
export default {
  name: "FormCliente",
  props: {
    cliente: Object,
  },
  data() {
    return {
      clienteData: {
        id: 0,
        nome: "",
        email: "",
        senha: "",
        telefone: "",
        cpf: "",
      },
      emailRegex: /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i,
      cpfRegex: /^\d{3}\.\d{3}\.\d{3}-\d{2}$/, // Adicione um regex para CPF válido
    };
  },
  created() {
    if (this.cliente) {
      this.clienteData = this.cliente;
    }
  },
  methods: {
    salvar() {
      this.$emit("salvarCliente", this.clienteData);
    },
    reset() {
      this.clienteData = {
        id: 0,
        nome: "",
        email: "",
        telefone: "",
        senha: "",
        cpf: "",
      };
    },
  },
};
</script>

<style></style>
