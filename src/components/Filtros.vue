<script setup>
    defineProps(['filtro'])
    defineEmits(['update:filtro'])


</script>
<template>
    <div class="contenedor sombra filtros">
        <form class="campo"> 
            <label for="filtro"> Filtrar Gastos</label>
            <select 
                id="filtro"
                :value="filtro"
                @input="$emit('update:filtro', $event.target.value)"
            >
                <option value="">-- Seleccione --</option>
                <option value="ahorro">Ahorro</option>
                <option value="comida">Comida</option>
                <option value="casa">Casa</option>
                <option value="ocio">Ocio</option>
                <option value="salud">Salud</option>
                <option value="suscripciones">Suscripciones</option>
            </select>
        </form>
    </div>



</template>
<style scoped>
    .filtros{
        margin-top: 10rem;
    }

    .filtro .campo{
        display: flex;
        align-items: center;
        gap: 2rem;   
    }

    .filtros label {
        font-size: 3rem;
        font-weight: 900;
        color: var(--gris-oscuro)
    }

    .filtros select{
        flex: 1;
        width: 100%;
        padding: 1rem;
        border: none;
        border-radius: 1rem;
        background-color: var(--gris-claro);
        text-align: center;

    }

    .filtros option {
        font-size: 2rem;
    }

</style>