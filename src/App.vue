<script setup>
  import { ref, computed, watch } from 'vue';
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
  tarefas.value.sort((a, b) => a.tarefa.localeCompare(b.tarefa).length)
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
const concluidas = ref (tarefaFiltrada.value.filter((item) => item.status == 'concluida').length);
const pendentes = ref (tarefaFiltrada.value.filter((item) => item.status == 'pendente').length);
watch(tarefas.value, ()=> {
  concluidas.value = tarefaFiltrada.value.filter((item) => item.status == 'concluida').length;
  pendentes.value = tarefaFiltrada.value.filter((item) => item.status == 'pendente').length;
})
watch(tarefaFiltrada, ()=> {
  concluidas.value = tarefaFiltrada.value.filter((item) => item.status == 'concluida').length;
  pendentes.value = tarefaFiltrada.value.filter((item) => item.status == 'pendente').length;
})
</script>

<template>
  <div class="container">
    <h1>Lista de Tarefas</h1>
    <input type="text" v-model="novaTarefa" placeholder="Adicionar Tarefa">
    <button @click="addTarefa()">Adicionar</button>
    <button @click="ordenarTarefa()">Ordenar</button>
    <ul>
      <li class="lista" v-for="item in tarefaFiltrada" :key="item.id" :class="{ concluida: item.status === 'concluida' }">
        {{ item.tarefa }}
          <div>
            <button @click="delTarefa(item)">Deletar</button>
            <button @click="editTarefa(item)">Editar</button>
            <button @click="concluirLista(item)">Concluida</button>
          </div>
        </li>
      </ul>
    <input type="text" v-model="filtro" placeholder="Filtrar Tarefa">
    <div class="contador">
      <p>Pendentes: {{ pendentes }}</p>
      <p>Concluidas: {{ concluidas }}</p>
    </div>
  </div>
</template>

<style scoped>
div.container {
  text-align: center;
}
div ul li {
  display: flex;
  justify-content: space-between;
}
div ul li div button {
  margin: 0 0 0 0;
  border: none;
}
div ul li div button:first-child {
  margin: 0 0 0 1vw;
}
div.container div.contador {
  gap: 10px;
  display: flex;
  justify-content: center;
}
.concluida {
  text-decoration: line-through;
  color: rgb(69, 197, 75);
}
li {
  color: rgb(207, 67, 67);
}
</style>
