<script setup>
  import { ref } from 'vue'
  const tarefas = ref([
  {
    id: 1,
    tarefa: 'Tarefa 1',
    status: 'concluida',
  },
  {
    id: 2,
    tarefa: 'Tarefa 2',
    status: 'concluida',
  },
  {
    id: 3,
    tarefa: 'Tarefa 3',
    status: 'pendente',
  },
  {
    id: 4,
    tarefa: 'Tarefa 4',
    status: 'pendente',
  },
])
const novaTarefa = ref ("");

function addTarefa(item) {
  tarefas.value.push({id: tarefas.value[tarefas.value.length -1].id + 1, tarefa: item, status: 'pendente'})
}
function delTarefa(item) {
  let i = tarefas.value.indexOf(item);
  tarefas.value.splice(i, 1);
}
function ordenarTarefa() {

}
</script>

<template>
  <div class="container">
    <h1>Lista de Tarefas</h1>
    <input type="text" v-model="novaTarefa">
    <button @click="addTarefa(novaTarefa)">Add-Tarefa</button>
    <button @click="ordenarTarefa()">Ordenar-Tarefa</button>
    <ul>
      <li v-for="item in tarefas" :key="item.id">
        {{ item.tarefa }}
        <button @click="delTarefa(item)">Del-Tarefa</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
</style>
