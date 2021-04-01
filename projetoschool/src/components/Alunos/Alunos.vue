<template>
  <div>
    <titulo text="Alunos" />
    <div>
      <input
        type="text"
        placeholder="Nome do Aluno"
        v-model="nome"
        @keyup.enter="addAluno()"
      />
      <button class="btn btnInput" @click="addAluno()">Adicionar</button>
    </div>

    <table>
      <thead>
        <th>Mat.</th>
        <th>Nome</th>
        <th>Opções</th>
      </thead>
      <tbody v-if="alunos.length">
        <tr v-for="(aluno, index) in alunos" :key="index">
          <td>{{ aluno.id }}</td>
          <td>{{ aluno.nome }} {{ aluno.sobrenome }}</td>
          <td>
            <button class="btn btn_Danger" @click="remover(aluno)">
              Remover
            </button>
          </td>
        </tr>
      </tbody>
      <tfoot v-else>
        Nenhum Aluno Encontrado
      </tfoot>
    </table>
  </div>
</template>

<script>
import Titulo from "../_share/Titulo";
import axios from "axios";
export default {
  components: {
    Titulo,
  },
  data() {
    return {
      titulo: "Aluno",
      nome: "",
      alunos: [],
    };
  },
  created() {
    axios.get("http://localhost:3000/alunos").then((response) => {
      this.alunos = response.data;
    });
  },

  props: {},
  methods: {
    addAluno() {
      let _aluno = {
        nome: this.nome,
        sobrenome: "",
      };
      axios.post("http://localhost:3000/alunos", _aluno).then((response) => {
        this.alunos.push(response.data);
        this.nome = "";
      });
    },
    remover(aluno) {
      let indice = this.alunos.indexOf(aluno);
      axios.delete(`http://localhost:3000/alunos/${aluno.id}`).then(() => {
        this.alunos.splice(indice, 1);
      });
    },
  },
};
</script>

<style scoped>
input {
  border: 0;
  padding: 20px;
  font-size: 1.3em;
  color: #687f7f;
  display: inline;
}
.btnInput {
  border: 0px;
  padding: 20px;
  font-size: 1.3em;
  display: inline;
  background-color: rgb(116, 115, 115);
}
.btnInput:hover {
  padding: 20x;
  margin: 0px;
  border: 0px;
}
</style>
