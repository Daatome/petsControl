<script setup>
  import { ref, reactive, watch, onMounted } from 'vue'
  import {uid} from 'uid'
  import Header from './components/Header.vue'
  import Formulario from './components/Formulario.vue'
  import Paciente from './components/Paciente.vue'


  const pacientes= ref([])
  const paciente= reactive({
    id:null,
    nombre:'',
    propietario:'',
    email:'',
    alta:'',
    sintomas:''
  })

  onMounted(()=>{
    pacientes.value= JSON.parse(localStorage.getItem('pacientes'))
  })

  watch(pacientes,()=>{
    guardaPacientes()
  },{
    deep:true
  })

  //guarda los pacientes en el local storage
  const guardaPacientes=()=>{
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
  }
  //guarda un paciente en el state
  const guardaPaciente=()=>{

    if(paciente.id){//si se tiene ya un id quiere decir que ya existe
      const {id}=paciente//destructuring
      const i= pacientes.value.findIndex((pacienteArray)=> pacienteArray.id===id )
      pacientes.value[i]={...paciente}//copy of the paciente
    }else{

      pacientes.value.push({
        ...paciente,
        id:uid()
      })
    }

    limpiarCampos()
    
  }

  const editarPaciente= (id)=>{
    //el cero lo hace para que solo nos retorne un elemento en lugar de un arreglo
    const pacienteEditar= pacientes.value.filter(pacienteArray=> pacienteArray.id===id)[0]

    Object.assign(paciente,pacienteEditar)
  }
  const eliminarPaciente= (id)=>{
    //retorna todos los pacientes que no tengan el id dado
    pacientes.value=pacientes.value.filter((pacienteArray)=> pacienteArray.id!==id)
  }

  const limpiarCampos= ()=>{
    paciente.nombre=''
    paciente.alta=''
    paciente.email=''
    paciente.propietario=''
    paciente.sintomas=''
    paciente.id=null
  }
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header/>

    <div class="md:flex justify-center">

      <Formulario
        v-model:nombre="paciente.nombre"
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:alta="paciente.alta"
        v-model:sintomas="paciente.sintomas"
        @guardaPaciente="guardaPaciente"
        @limpiar-campos="limpiarCampos"
        :id="paciente.id"
      />

      <div class="md:w-1/2 overflow-y-scroll  ">
        <h3 class="text-3xl text-center font-black">Administra a tus Pacientes</h3>
        <div v-if="pacientes.length===0">
          <p class="text-xl text-center">No tienes pacientes registrados actualmente</p>
        </div>
        <div v-else>
          <Paciente 
            v-for="paciente in pacientes"
            :paciente="paciente"
            @editar-paciente="editarPaciente"
            @eliminar-paciente="eliminarPaciente"

          />
        </div>

      </div>
    </div>
  </div>
</template>


