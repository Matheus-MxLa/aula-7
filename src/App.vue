<script setup>
  import { ref, computed } from 'vue';
  const tarefas = ref([
  {
    id: 1,
    tarefa: '1 Supino Inclinado',
    status: 'concluida',
  },
  {
    id: 2,
    tarefa: '2 Crucifixo no Voador',
    status: 'concluida',
  },
  {
    id: 3,
    tarefa: '3 Supino Reto',
    status: 'pendente',
  },
  {
    id: 4,
    tarefa: '4 Elevação Lateral',
    status: 'pendente',
  },
  {
    id: 5,
    tarefa: '5 Desenvolvimento Halter',
    status: 'pendente',
  },
  {
    id: 6,
    tarefa: '6 Triceps Frances',
    status: 'pendente',
  },
  {
    id: 7,
    tarefa: '7 Triceps na Polia',
    status: 'pendente',
  },
])
const novaTarefa = ref ('');
const posiAlterada = ref (null);

function addTarefa() {
  if (novaTarefa.value.trim().length >= 5) {
    if (posiAlterada.value !== null) {
      tarefas.value[posiAlterada.value].tarefa = novaTarefa.value;
      posiAlterada.value = null;
    }
    else {
      tarefas.value.push({
        id: tarefas.value.length + 1,
        tarefa: novaTarefa.value,
        status: 'pendente'
      });
    }
    novaTarefa.value = '';
  }
}
function delTarefa(item) {
  let i = tarefas.value.indexOf(item);
  tarefas.value.splice(i, 1);
}
function editTarefa(item) {
  posiAlterada.value = tarefas.value.indexOf(item);
  novaTarefa.value = item.tarefa;
}

function concluirLista(item) {
  item.status = item.status === 'concluida' ? 'pendente' : 'concluida';
}
function ordenarTarefa () {
  tarefas.value.sort((a, b) => a.tarefa.localeCompare(b.tarefa))
}
const filtro = ref ('')
const tarefaFiltrada = computed(() => {
  if (filtro.value.trim().length > 0) {
    return tarefas.value.filter(item => item.tarefa.includes(filtro.value));
  }
  else {
    return tarefas.value
  }
})
</script>

<template>
  <div class="container">
    <h1>Lista de Tarefas</h1>
    <input type="text" v-model="novaTarefa">
    <button @click="addTarefa()">Adicionar</button>
    <button @click="ordenarTarefa()">Ordenar</button>
    <ul>
      <li v-for="item in tarefaFiltrada" :key="item.id" :class="{ concluida: item.status === 'concluida' }">
        {{ item.tarefa }}
        <button @click="delTarefa(item)">Deletar</button>
        <button @click="editTarefa(item)">Editar</button>
        <button href="#" @click="concluirLista(item)">Concluida</button>
      </li>
    </ul>
    <input type="text" v-model="filtro">
  </div>
</template>

<style scoped>
.concluida {
  text-decoration: line-through;
  color: rgb(69, 197, 75);
}
li {
  color: rgb(207, 67, 67);
}
</style>
