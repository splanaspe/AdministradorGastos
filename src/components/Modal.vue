<script setup>

    import {ref} from  'vue'
    import cerrarModal from './../assets/img/cerrar.svg'
    import Alerta from './Alerta.vue';

    const error=ref('')

    const emit = defineEmits(['ocultar-modal', 'eliminar-gasto', 'guardar-gasto','update:nombre', 'update:cantidad', 'update:categoria']);

    const props = defineProps({
        modal: {
            type: Object,
            required:true
        },
        nombre: {
            type: String,
            required: true
        },
        cantidad: {
            type: [String, Number],
            required: true
        },
        nombre: {
            type: String,
            required: true
        },
        categoria: {
            type: String,
            required: true
        },
        disponible:{
            type: Number,
            required: true
        },
        id: {
            type: [String, null],
            required: true
        }
    })

    const oldCantidad = props.cantidad;

    const agregarGasto = () => {
        // Validar
        const {cantidad, categoria, nombre, disponible, id} = props
        if([nombre,cantidad,categoria].includes('')){
            error.value= 'Todos los campos son obligatorios'
            setTimeout( () => {
                error.value=""
            },2000)
            return
        }

        if(cantidad <= 0){
            error.value="Cantidad no válida"
            setTimeout( () => {
                error.value=""
            },2000)
            return
        }
        console.log("Tienes disponible: "+disponible)
        //Validar que el usuario no gaste mas de lo disponible
        if(id){
            if(cantidad > oldCantidad + disponible){
                error.value="Has excedido el presupuesto"
                setTimeout( () => {
                    error.value=""
                },2000)
                return
            }
        } else{
            if(cantidad > disponible){
                error.value="Has excedido el presupuesto"
                setTimeout( () => {
                    error.value=""
                },2000)
                return
            }
        }
        
        emit('guardar-gasto')
    }

</script>

<template>
    <div class="modal">
        <div class="cerrar-modal">
            <img :src="cerrarModal"
            @click="$emit('ocultar-modal')"> 
        </div>
        <div 
            class="contenedor contenedor-formulario"
            :class="[modal.animar ? 'animar' : 'cerrar']"
        >
            <form 
                class="nuevo-gasto"
                @submit.prevent="agregarGasto"
            > 
                <Legend>
                    {{ id ? 'Guardar Cambios' : 'Añadir Gasto' }}
                </Legend>
                <Alerta v-if="error"> 
                    {{ error }}
                </Alerta>
                <div class="campo">
                    <label for="nombre"> Nombre Gasto </label>
                    <input
                        type="text"
                        id="nombre"
                        placeholder="Añade el nombre del gasto"
                        :value="nombre"
                        @input="$emit('update:nombre', $event.target.value)"
                    >
                </div>
                <div class="campo">
                    <label for="cantidad"> Cantidad </label>
                    <input
                        type="number"
                        id="cantidad"
                        placeholder="Añade la cantidad del gasto"
                        :value="cantidad"
                        @input="$emit('update:cantidad', +$event.target.value)"
                    >
                </div>
                <div class="campo">
                    <label for="categoria">Categoria: </label>
                    <select 
                        id="categoria"
                        :value="categoria"
                        @input="$emit('update:categoria', $event.target.value)"
                    >
                        <option vale="">-- Seleccione --</option>
                        <option value="ahorro">Ahorro</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="suscripciones">Suscripciones</option>
                    </select>
                </div>
                <input type="submit"
                    :value="[id ? 'Guardar Cambios' : 'Añadir Gasto']"
                > 
            </form>

            <button
            v-if="id"
                @click="$emit('eliminar-gasto',id)"
                type="button"
                class="btn-eliminar"
            >
                Eliminar Gasto
            </button>
        </div>
    </div>
</template>

<style scoped>
    .modal{
        position:absolute;
        background-color: rgb(0 0 0 /0.9);
        top:0;
        left:0;
        right:0;
        bottom: 0;
    }

    .cerrar-modal {
        position: absolute;
        right: 3rem;
        top: 3rem;
    }

    .contenedor-formulario{
        transition-property: all;
        transition-duration: 300ms;
        transition-timing-function: ease-in;
    }

    .contenedor-formulario.animar{
        opacity: 1;
    }

    .contenedor-formulario.cerrar{
        opacity: 0;
    }

    .cerrar-modal img {
        width: 3rem;
        cursor: pointer;
    }   

    .nuevo-gasto{
        margin: 10rem auto 0 auto;
        display: grid;
        gap:2rem;
    }

    .nuevo-gasto legend {
        text-align: center;
        color: var(--blanco);
        font-size: 3rem;
        font-weight: 700;
    }

    .campo {
        display:grid;
        gap: 2rem;
    }

    .nuevo-gasto input,
    .nuevo-gasto select {
        background-color: var(--gris-claro);
        border-radius: 1rem;
        padding: 1rem;
        border:none;
        font-size: 2.2rem;
    }

    .nuevo-gasto label {
        color: var(--blanco);
        font-size:3rem;
    }

    .nuevo-gasto input[type="submit"] {
        background-color: var(--azul);
        color: var(--blanco);
        font-weight: 700;
        cursor: pointer;
    }

    .btn-eliminar{
        border:none; 
        padding:1rem;
        width: 100%;
        background-color: #ef4444;
        font-weight: 700;
        font-size: 1.2rem;
        color: var(--blanco);
        margin-top: 10rem;
        cursor: pointer;
    }

</style>