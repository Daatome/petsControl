
<script setup>
import { computed } from 'vue';

const props= defineProps({
    alerta:{
        type:Object,
        required:true
    }
})
const isError= computed(()=>{
    return props.alerta.tipo ==='error'
})

</script>

<template>
    <div class="text-xl text-center text-white rounded-lg p-3 m-2"
        :class="[isError ? 'bg-red-500 ': 'bg-green-500']"
    >
       {{ alerta.mensaje}}
    </div>
</template>
