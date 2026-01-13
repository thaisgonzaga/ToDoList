<script setup>

import { ref } from 'vue';

const props = defineProps({
  texto: String,
  concluida: Boolean,
  criadaEm: Date,
  prazo: String
})

const editando = ref(false);
const textoEditado = ref(props.texto);
const prazoEditado = ref(props.prazo);

const emit = defineEmits(['remover', 'concluir', 'editar']);

function removerTarefa(){
  emit('remover');
}

function concluirTarefa(){
  emit('concluir');
}

function editarTarefa(){
  emit('editar', { novoTexto: textoEditado.value, novoPrazo: prazoEditado.value });
}

function toggleEditando(){
  editando.value = !editando.value;
}

function formatarData(data) {
  if (!data) return ''
  const dataObj = new Date(data)
  return dataObj.toLocaleDateString('pt-BR')
}


</script>

<template>
    <li class="task" :class="{ riscada: concluida }" v-if="!editando">
        <p class="task-text">{{texto}}</p>
        <div class="task-meta">
        <p class="task-text"> <small> Criada em: {{ formatarData(criadaEm) }} </small> </p>
        </div>
        <p class="task-text"> <small> Prazo: {{ formatarData(prazo)}} </small> </p>
        <div class="task-actions"> 
        <button @click="removerTarefa" class="button-delete"><i class="fa-solid fa-x delete"></i></button>
        <button @click="toggleEditando" class="button-edit"><i class="fa-solid fa-pen editar"></i></button>
        <input type="checkbox" @change="concluirTarefa" />
        </div>
    </li>
    <li v-else class="task">
        <input v-model="textoEditado" type="text" />
        <input v-model="prazoEditado" type="date" />
        <div class="button-salvar-cancelar">
        <button @click="() => { editarTarefa(); toggleEditando(); }"> Salvar </button>
        <button @click="toggleEditando()"> Cancelar </button>
        </div>
    </li>
</template>

<style scoped>
.delete{
    color: white;
}

.button-delete{
    background-color: #ef4444;
    color: white;
}

.button-delete:hover{
    background-color: #dc2626;
    cursor: pointer;
}

.button-edit{
    background-color: #3b82f6;
    color: white;
}

.button-edit:hover{
    background-color: #2563eb;
    cursor: pointer;
}

.riscada{
    text-decoration: line-through;
    opacity: 0.6;
}

.editar{
    color: white;
}

.button-salvar-cancelar{
    display: flex;
    gap: 10px;
    margin-top: 10px;
  
}
</style>