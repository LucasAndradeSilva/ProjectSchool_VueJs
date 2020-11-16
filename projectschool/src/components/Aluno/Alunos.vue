<template>
  <div>  
    <Titulo texto="Alunos"/>
    <input type="text" placeholder="Nome do Aluno" v-model="nome" @keyup.enter="addAluno()" />  
    <button class="btnInput" @click="addAluno()">Adicionar</button>
    <table>
      <thead>
        <th>Mat.</th>
        <th>Nome</th>
        <th>Sobrenome</th>
        <th>Opções</th>
      </thead>
      <tbody v-if="alunos.length">
        <tr v-for="(aluno, index) in alunos" :key="index">
          <td>{{ aluno.id }}</td>
          <td>{{ aluno.nome }}</td>
          <td>{{ aluno.sobrenome }}</td>
          <td>
            <button class="btn btn_Danger" @click="remover(aluno)">Remover</button>
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
import Titulo from '../_shared/Titulo';

export default {  
  components: {
    Titulo
  },
  data() {
    return{
      titulo: 'Aluno',
      nome: '',
      alunos: []
    }
  },
  created() {
    this.$http.get('http://localhost:3000/alunos').then(res => res.json()).then(alunos => this.alunos = alunos)
  },
  props: {    
  },
  methods: {
    addAluno(){
      let _aluno = {        
        nome: this.nome.split(' ')[0],
        sobrenome: this.nome.split(' ')[1],
        id: this.alunos.length+1
      }

      if(_aluno.nome != ""){        
        this.$http.post('http://localhost:3000/alunos', _aluno).then(res => res.json());
        this.alunos.push(_aluno);
        this.nome = '';
      }
      else{
        alert("Campo vazio!")
      }
    },
    remover(aluno){
      this.$http.delete(`http://localhost:3000/alunos/${aluno.id}`).then(() => {
        let indice = this.alunos.indexOf(aluno);
        this.alunos.splice(indice, 1);
      });

 
    }
  },
}
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Montserrat:400,700");

body{
  background-color: #eee;
  font-family: "Montserrat", sans-serif;
  display: grid;
  justify-items: center;
}

body, html {
  margin: 0;
  height: 100%;
}

#app {

}

.btnInput:hover{
  text-shadow: 1px 1px 1px #000;
  border-bottom: 3px solid #000;
  margin-top: 7px;

}

.btnInput{
  border: 1px solid #000;
  padding: 7px;
  font-size: 1.3em;
  display: inline;
  background-color: rgb(116, 115, 115);
  color: #fff;
  border-radius: 5px;
  background-color: #99dc14;
}

input{    
  margin: 10px;
  border: 0;
  padding: 7px;
  font-size: 1.3em;
  color: #687f7f;
  display: inline;
  border-radius: 5px;
}

.btn_Danger{
  background-color: #fa4430;
}

.btn_Success{
  background-color: #99dc14;
}

.btn{  
  padding: 10px 20px;
  border: 1px solid #000;
  cursor: pointer;
  color: #fff;
  font-weight: bold;
  border-radius: 5px;
  border-bottom: 3px solid #000;  
}

.btn:hover{
  text-shadow: 1px 1px 1px #000;
  border-bottom: 1px solid #000;
  margin-top: 3px;
}

table {
  margin: 0px;
  padding: 0px;
  list-style-type: none;
  width: 100%;
}
table tr td{
  padding: 20px;
  font-size: 1.3em;
  background-color: #e0edf4;
  margin-bottom: 2px;
  color: #3e5252;
}
table thead th{
  background-color: rgb(184, 208, 216) !important;
  font-size: 1.2em;
  padding: 10px 0px;
  text-align: center !important;
}
.colPequeno {
  width: 5%;
}
</style>
