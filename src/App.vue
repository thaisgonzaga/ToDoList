<script setup>
import TodoItem from './components/TodoItem.vue'
import { ref, computed} from 'vue';

const texto = ref('');
const tarefas = ref([]);
const filtro = ref('todas');
const prazo = ref('');



function adicionarTarefa(event){
  event.preventDefault();
  if(texto.value !== ''){
    tarefas.value.push({ texto: texto.value, concluida: false, criadaEm: new Date(), prazo: prazo.value });
    texto.value = '';
    prazo.value = '';
  } else{
    alert('Por favor, insira uma tarefa válida.');
  }
}

function removerTarefa(index){
  tarefas.value.splice(index, 1);
}

function concluirTarefa(index){
  tarefas.value[index].concluida = !tarefas.value[index].concluida;
}

const tarefasFiltradas = computed (() => {
  if (filtro.value === 'concluidas') {
    return tarefas.value.filter(tarefa => tarefa.concluida);
  } else if (filtro.value === 'pendentes') {
    return tarefas.value.filter(tarefa => !tarefa.concluida);
  } else {
    return tarefas.value;
  }
})

const contadorTarefas = computed(() => {
  return tarefas.value.length;
});

const contadorTarefasConcluidas = computed(() => {
  return tarefas.value.filter(tarefa => tarefa.concluida).length;
});

const contadorTarefasPendentes = computed(() => {
  return contadorTarefas.value - contadorTarefasConcluidas.value;
})

function editarTarefa(novoTexto, novoPrazo, index){
  if(novoTexto.trim() !== ''){
    tarefas.value[index].texto = novoTexto;
    tarefas.value[index].prazo = novoPrazo;
  } else {
    alert('O texto da tarefa não pode ser vazio.');
  }
}



</script>

<template>
  <div class="app">
  <h1>Minha To-Do List</h1>

  <form @submit="adicionarTarefa">
    <div class="div-input section">
      <input v-model="texto" type="text" placeholder="Digite uma tarefa">
      <input v-model="prazo" type="date">
    </div>

    <div class="div-button">
    <button type="submit"> Adicionar </button>
    </div>
  </form>

  <div>
    <ul>
      <TodoItem v-for="(tarefa, index) in tarefasFiltradas" :key="index" :texto="tarefa.texto" :concluida="tarefa.concluida" :criadaEm="tarefa.criadaEm" :prazo="tarefa.prazo"
      @remover="removerTarefa(index)"
      @concluir="concluirTarefa(index)" 
      @editar="editarTarefa($event.novoTexto, $event.novoPrazo, index)"
      />
    </ul>
  </div>

  <div class="filters section">
    <button :class="{ active: filtro === 'concluidas' }" @click="filtro = 'concluidas'"> Tarefas concluidas </button>
    <button :class="{ active: filtro === 'pendentes' }" @click="filtro = 'pendentes'"> Tarefas pendentes </button>
    <button :class="{ active: filtro === 'todas' }" @click="filtro = 'todas'"> Todas as tarefas </button>
  </div>

  <div class="summary">
    <div class="summary-itens">
    <p class="task-text"> <strong> Total de tarefas: </strong> <span> {{contadorTarefas}} </span></p>
    </div>
    <div class="summary-itens">
    <p class="task-text"> <strong> Tarefas concluidas: </strong> <span> {{contadorTarefasConcluidas}} </span></p>
    </div>
    <div class="summary-itens">
    <p class="task-text"> <strong> Tarefas pendentes: </strong> <span> {{contadorTarefasPendentes}} </span></p>
    </div>
  </div>
  </div>

</template>

<style scoped>
.div-input{
  margin-bottom: 1rem;
}

.div-button{
  margin-bottom: 1rem;
}
</style>
