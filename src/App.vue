<script setup>

import { ref, reactive, watch, onMounted } from 'vue';
import { uid } from 'uid'
import Header from './components/Header.vue';
import Formulario from './components/Formulario.vue'
import Paciente from './components/Paciente.vue'

const pacientes = ref([])

const paciente = reactive({
  id: null,
  nombre: '',
  propietario: '',
  email: '',
  alta: '',
  sintomas: '',
})

watch(pacientes, () => {
  guardarLocalStorage()
}, {
  //incluso los cambios de cada uno de los objetos los valide
  deep: true
})

onMounted(() => {
  const pacientesStorage = localStorage.getItem('pacientes')

  if(pacientesStorage){
    pacientes.value = JSON.parse(pacientesStorage)
  }
})

const guardarLocalStorage = () => {
  localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
}

const guardarPaciente = () => {

  if(paciente.id){
    
    const {id} = paciente

    //devuelve la posicion del elemento que se busca a traves del id
    const i = pacientes.value.findIndex((pacienteState) => pacienteState.id === id)

    //edita los datos del paciente
    pacientes.value[i] = {...paciente}

  } else{
    //guarda un paciente en el arreglo de pacientes y crea un objeto nuevo de paciente
    pacientes.value.push({
      ...paciente, 
      id: uid()
    })
  }


  paciente.nombre = '' 
  paciente.propietario = ''
  paciente.email = ''
  paciente.alta = ''
  paciente.sintomas = ''
  paciente.id = null
}

const actualizarPaciente = (id) => {
  const pacienteEditar = pacientes.value.filter(paciente => paciente.id === id)[0]

  Object.assign(paciente, pacienteEditar)
}

const eliminarPaciente = (id) =>{
  pacientes.value = pacientes.value.filter(paciente => paciente.id !== id)
}


</script>

<template>
  <div class="container mx-auto mt-10">
    <Header />

    <div class="mt-7 md:flex">

      <Formulario 
        v-model:nombre="paciente.nombre" 
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email" 
        v-model:alta="paciente.alta" 
        v-model:sintomas="paciente.sintomas" 
        @guardar-paciente="guardarPaciente"
        :id="paciente.id"
      />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">

        <h3 class="font-black text-3xl text-center">
          Administra tus pacientes
        </h3>

        <div class="" v-if="pacientes.length > 0">
          <Paciente 
            v-for="paciente in pacientes"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>

        <p v-else class="mt-10 text-2xl text-center">
          No hay pacientes
        </p>


      </div>

    </div>

  </div>
</template>


