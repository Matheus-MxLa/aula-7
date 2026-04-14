<script setup>
defineProps (['id', 'tarefa', 'status'])
defineEmits (['cliqueDelete', 'cliqueEdit', 'cliqueConcluir', 'marcarconcluida'])
</script>

<template>
  <li>
    <span @click="$emit('marcarconcluida')" :class="{ concluida: status === 'concluida' }">{{ tarefa }}</span>
    <button @click="$emit('cliqueDelete')">Deletar</button>
    <button @click="$emit('cliqueEdit')">Editar</button>
    <button @click="$emit('cliqueConcluir')">Concluida</button>
  </li>
</template>

<style scoped>
div ul li {
  display: flex;
  justify-content: space-between;
}
.concluida {
  text-decoration: line-through;
  color: rgb(69, 197, 75);
}
span {
  cursor: pointer;
}
</style>
