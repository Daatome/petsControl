<script setup>
import {reactive, computed} from 'vue'
import Alerta from './Alerta.vue'


const emit= defineEmits(['update:nombre','update:email','update:propietario','update:alta','update:sintomas','guarda-paciente','limpiar-campos'])
const props= defineProps({
    nombre:{
        type:String,
        required:true
    },
    email:{
        type:String,
        required:true
    },
    propietario:{
        type:String,
        required:true
    },
    alta:{
        type:String,
        required:true
    },
    sintomas:{
        type:String,
        required:true
    },
    id:{
        type:[String, null],
        required:true
    }

})

const alerta= reactive({
    tipo:'',
    mensaje:''
})


const validar= ()=>{

    //if any field of the object paciente has the value '' means it is empty so we can´t register it
    if(Object.values(props).includes('')){
        alerta.tipo= 'error'
        alerta.mensaje='Todos los campos son obligatorios'
        return
    }
    emit('guarda-paciente')
    alerta.tipo='exito'
    alerta.mensaje='Paciente almacenado correctamente'

    setTimeout(() => {
        Object.assign(alerta,{
            tipo:'',
            mensaje:''
        })
        //or 
        //alerta.tipo=''
        //alerta.mensaje=''
    }, 3000)
}

const editando = computed(()=>{
    return props.id
})




</script>
<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl">Seguimiento de pacientes</h2>
        <p class="text-lg mt-5  mb-10">Añade pacientes y <span class="text-amber-500 font-bold">adminístralos</span> </p>
        <Alerta 
        v-if="alerta.mensaje!==''"
        :alerta="alerta"
        />
        <form 
        @submit.prevent="validar"
        class="bg-white shadow-lg rounded-lg py-10 px-5 mb-10">

            <div class="mb-5">
                <label for="mascota" class="block text-gray-700 font-bold uppercase">Nombre de la mascota</label>
                <input type="text" id="mascota" placeholder="nombre de la mascota" class="border-2 w-full p-2 mt-2 placeholder-gray-300  rounded-lg"
                @input="$emit('update:nombre',$event.target.value)"
                :value="nombre">

            </div>
            <div class="mb-5">
                <label for="propietario" class="block text-gray-700 font-bold uppercase">propietario</label>
                <input type="text" id="propietario" placeholder="nombre del dueño" class="border-2 w-full p-2 mt-2 placeholder-gray-300  rounded-lg"
                @input="$emit('update:propietario',$event.target.value)"
                :value="propietario">

            </div>
            <div class="mb-5">
                <label for="email" class="block text-gray-700 font-bold uppercase">email </label>
                <input type="email" id="email" placeholder="email del propietario" class="border-2 w-full p-2 mt-2 placeholder-gray-300  rounded-lg"
                @input="$emit('update:email',$event.target.value)"
                :value="email">

            </div>
            <div class="mb-5">
                <label for="alta" class="block text-gray-700 font-bold uppercase">Fecha de alta</label>
                <input type="date" id="alta" placeholder="Fecha de alta" class="border-2 w-full p-2 mt-2 placeholder-gray-300  rounded-lg"
                @input="$emit('update:alta',$event.target.value)"
                :value="alta">

            </div>
            <div class="mb-5">
                <label for="sintomas" class="block text-gray-700 font-bold uppercase">Sintomas</label>
                <textarea id="sintomas" placeholder="Describe los sintomas" class="border-2 w-full p-2 mt-2 placeholder-gray-300  rounded-lg"
                @input="$emit('update:sintomas',$event.target.value)"
                :value="sintomas"></textarea>

            </div>

            <input type="submit"
            class="bg-amber-500 text-center font-black block w-full rounded-lg hover:bg-amber-600 p-5 m-3 text-gray-700 text-xl " :value="editando ? 'Editar Paciente': 'Registrar Paciente'">

            <p class="bg-gray-500 text-center font-black block w-full rounded-lg hover:bg-gray-700 p-5 m-3 text-white text-xl cursor-pointer " 
                @click="$emit('limpiar-campos')"
            >Limpiar Campos</p>


        </form>

    </div>
</template>


