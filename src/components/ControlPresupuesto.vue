<script setup>
    import "vue3-circle-progress/dist/circle-progress.css";
    import CircleProgress from 'vue3-circle-progress';
    import { formatearCantidad } from '../helpers';
    import {computed} from 'vue'
    
    const props = defineProps({
        presupuesto:{
            type: Number, 
            required: true
        },
        disponible:{
            type: Number, 
            required: true
        },
        gastado: {
            type: Number,
            required: true
        }
    })

    defineEmits(['reset-app'])

    const porcentaje = computed( () =>{
        return parseInt(((props.presupuesto-props.disponible)/props.presupuesto)*100)
    })

</script>
<template>
    <div class="dos-columnas">
        <div class="contenedor-grafico">
            <CircleProgress
                :percent="porcentaje"
                :size="250"
                :border-width="30"
                :border-bg-width="30"
                
            > 
            </CircleProgress>
        </div>
        <div class="contenedor-presupuesto">
            <button @click="$emit('reset-app')" class="reset-app"> 
                Resetear App
            </button>
            <p> 
                <span> Presupuesto : </span>
                {{ formatearCantidad(presupuesto) }}
            </p>
            <p> 
                <span> Disponible : </span> 
                {{ formatearCantidad(disponible) }}
            </p>
            <p> 
                <span> Gastado : </span> 
                {{ formatearCantidad(gastado) }}
            </p>
        </div>
    </div>
</template>


<style scoped>

    .dos-columnas{
        display:flex;
        flex-direction: column;
    }

    .dos-columnas > :first-child{
        margin-bottom: 3rem;
    }

    @media (min-width: 768px){
        .dos-columnas{
            flex-direction: row;
            gap: 4rem;
            align-items: center;
        }

        .dos-columnas > :first-child{
            margin-bottom: 0;
        }
    }

    .reset-app{
        background-color: #DB2777;
        border:none;
        padding: 1rem;
        width: 100%;
        color:white;
        font-weight: 900;
        text-transform: uppercase;
        border-radius: 1rem;
        transition-property: background-color;
        transition-duration: 300ms;
    }

    .reset-app:hover{
        background-color: #ad1b5d;
        border:none;
        padding: 1rem;
        width: 100%;
        cursor:pointer;
    }

    .contenedor-presupuesto{
        width: 100%;
    }

    .contenedor-presupuesto p {
        font-size: 2.4rem;
        text-align: center;
        color: var(--gris-oscuro)
    }
    @media (min-width: 768px){
        .contenedor-presupuesto p {
        font-size: 2.4rem;
        text-align: left;
        }   
    }

    .contenedor-presupuesto span{
        font-weight: 900;
        color: var(--azul)
    }

</style>