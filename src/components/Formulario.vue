<script setup>

    import { reactive,computed } from 'vue';
    import Alerta from './Alerta.vue'

    const alerta = reactive({
        tipo: '',
        mensaje: ''
    })

    const emit = defineEmits([
        'update:nombre',
        'update:propietario',
        'update:email',
        'update:alta',
        'update:sintomas',
        'guardar-paciente'
    ])

    const props = defineProps({
        nombre:{
            type: String,
            required: true
        },
        propietario:{
            type: String,
            required: true
        },
        email:{
            type: String,
            required: true
        },
        alta:{
            type: String,
            required: true
        },
        sintomas:{
            type: String,
            required: true
        },
        id:{
            type: [String,null],
            required: true
        },
    })

    const validar = () =>{

        //convierte el objeto a arreglo y con la función include...
        //...verifica si están vacios los atributos
        if(Object.values(props).includes('')){
            alerta.mensaje = "Todos los campos son obligatorios"
            alerta.tipo = "error"
            return 
        }

        //ejecuta la función guardarPaciente de App.vue
        emit('guardar-paciente')

        alerta.mensaje = 'Paciente agregado correctamente'
        alerta.tipo = 'success'

        setTimeout(() => {
            Object.assign(alerta,{
                tipo: '',
                mensaje: ''
            })
        }, 4000)


    }

    const editando = computed(() => {
        return props.id
    })

</script>

<template>
    <div class="md:w-1/2">

        <p class="text-lg mt-5 text-center mb-5">
            Añade pacientes:
            <span class="text-indigo-600 font-bold">
                Adminístralos
            </span>
        </p>

        <Alerta 
            v-if="alerta.mensaje"
            :alerta="alerta"
        />

        <form 
            class="bg-white shadow-md rounded-lg py-10 px-5 mb-10 mx-10"
            @submit.prevent="validar"
            >

                <!-- Mascota -->
                <div class="mb-5">
                    <label
                        for="mascota"
                        class="block text-gray-700 uppercase font-bold"
                    >
                        Nombre mascota
                    </label>

                    <input 
                        id="mascota"
                        type="text"
                        placeholder="Nombre de la mascota"
                        class="border-2 w-full p-2 my-2 placeholder-gray-400 rounded-md"
                        :value="nombre"
                        @input="$emit('update:nombre', $event.target.value)"  
                    >
                </div>

                <!-- Propietario -->
                <div class="mb-5">
                    <label
                        for="propietario"
                        class="block text-gray-700 uppercase font-bold"
                    >
                        Propietario
                    </label>

                    <input 
                        id="propietario"
                        type="text"
                        placeholder="Nombre del propietario"
                        class="border-2 w-full p-2 my-2 placeholder-gray-400 rounded-md"
                        :value="propietario"
                        @input="$emit('update:propietario', $event.target.value)" 
                    >
                </div>

                <!-- Email -->
                <div class="mb-5">
                    <label
                        for="email"
                        class="block text-gray-700 uppercase font-bold"
                    >
                        Correo electrónico
                    </label>

                    <input 
                        id="email"
                        type="email"
                        placeholder="Ingrese su correo"
                        class="border-2 w-full p-2 my-2 placeholder-gray-400 rounded-md"
                        :value="email"
                        @input="$emit('update:email', $event.target.value)"
                    >
                </div>
            
                <!-- Alta (fecha) -->
                <div class="mb-5">
                    <label
                        for="alta"
                        class="block text-gray-700 uppercase font-bold"
                    >
                        Correo electrónico
                    </label>

                    <input 
                        id="alta"
                        type="date"
                        class="border-2 w-full p-2 my-2 placeholder-gray-400 rounded-md"
                        :value="email"
                        @input="$emit('update:alta', $event.target.value)"
                    >
                </div>

                <!-- Sintomas -->
                <div class="mb-5">
                    <label
                        for="sintomas"
                        class="block text-gray-700 uppercase font-bold"
                    >
                        Síntomas
                    </label>

                    <textarea
                        id="sintomas"
                        placeholder="Describe los síntomas"
                        class="border-2 w-full p-2 my-2 placeholder-gray-400 rounded-md"
                        :value="sintomas"
                        @input="$emit('update:sintomas', $event.target.value)"

                    ></textarea>

                </div>

                <input 
                    type="submit"
                    :value="[editando ? 'Guardar cambios' : 'Registrar paciente']"
                    class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer
                    transition-colors">

        </form>

    </div>
</template>


