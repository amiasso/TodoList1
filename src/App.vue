<script setup>
import { reactive } from 'vue';

const estado = reactive({
  filtro: 'todas',

  tarefaTemp: '',

  Tarefas:[
    {
      titulo: 'Estudar ES6',
      finalizada: false,
    },
    {
      titulo: 'Estudar SaSS',
      finalizada: false,
    },
    {
      titulo: 'Ir para academa',
      finalizada: true,
    }
  ]
})

const getTarefasPendentes = () => {
  return estado.Tarefas.filter(tarefa => !tarefa.finalizada)
}
const getTarefasFinalizadas = () => {
  return estado.Tarefas.filter(tarefa => tarefa.finalizada)
}
const getTrefasFiltradas = () => {
  const {filtro} = estado;

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
        default:
          return estado.Tarefas;
  }
  
}

const cadastrarTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }
  estado.Tarefas.push(tarefaNova);
  estado.tarefaTemp = '';
}
</script>

<template>
<div class="conatiner">
  <header class="p-5 mb-4 mt-4 bg-light rounded-3">
    <h1>Minhas Tarefas</h1>
    <p>Você Possui {{ getTarefasPendentes().length }} Tarefas Pendentes</p>
  </header>
</div>
<form @submit.prevent="cadastrarTarefa">
  <div class="row">
    <div class="col">
      <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite sua tarefa" class="form-control">
    </div>
    <div class="col md-1">
      <button type="submit" class="btn btn-primary">Cadastrar</button>
    </div>
   <div class="col-md-2">
    <select @change="evento => estado.filtro =evento.target.value" class="for-control">
      <option value="todas">Todas tarefas</option>
      <option value="pendentes">Pendentes</option>
      <option value="finalizadas">Finalizadas</option>
    </select>
   </div>
  </div>
</form>
<ul class="list-group mt-4">
  <li class="list-group-item" v-for="tarefa in getTrefasFiltradas()">
    <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
    <label :class="{ done: tarefa.finalizada}" class="ms-3" :for="tarefa.titulo">{{ tarefa.titulo }}</label>
  </li>

</ul>
</template>

<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>
