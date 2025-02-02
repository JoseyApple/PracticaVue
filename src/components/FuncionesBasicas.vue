<template>
  <header id="header">
    <div id="header-left">
      <h1>{{ librosDisponibles }} Libros disponibles</h1>
      <div id="filter-container">
        <select v-model="genero">
          <option value="todas">Todas</option>
          <option value="Fantasía">Fantasía</option>
          <option value="Ciencia ficción">Ciencia ficción</option>
          <option value="Terror">Terror</option>
          <option value="Zombies">Zombies</option>
        </select>

        <div id="page-slider">
          <label for="pageRange">Páginas: 10 - {{ maximoPaginas }}</label>
          <input type="range" id="pageRange" v-model="maximoPaginas" :min="10" :max="1500" step="10" />
        </div>
        <div id="search-bar">
          <input
            type="text"
            v-model="inputBusqueda"
            placeholder="🔍 Buscar libros por título..."
            />
          </div>
      </div>
    </div>
  </header>
</template>

<script setup>
import { ref, watch, defineEmits } from "vue";

const emit = defineEmits(["actualizarFiltros"]);

const genero = ref("todas");
const maximoPaginas = ref(1000);
const inputBusqueda = ref("");

// Necesito el número de libros disponibles para que renderice la cantidad en el header.
defineProps({
  books: Array,
  librosDisponibles: Number, // Recibimos librosDisponibles como número
});


//Watch sirve para que observe cambios en las variables y llame a la funcion emit
watch(
  () => [genero.value, maximoPaginas.value, inputBusqueda.value],
  () => {
    emit("actualizarFiltros", { //Actualiza los datos actualizados a otro componente..
      genero: genero.value,
      maximoPaginas: maximoPaginas.value,
      inputBusqueda: inputBusqueda.value
    });
  }
);
</script>


