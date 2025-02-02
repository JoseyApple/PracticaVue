<template>
  <aside id="lectura-container">
    <h2 class="lectura-title">Lista de lectura</h2>
    <div id="lectura">
      <!-- Si la lista de lectura esta vacía, no se muestran libros. Y indica en el contenedor que no hay libros en la lista de lectura -->
      <div v-if="listaDeLectura.length === 0" class="no-books">
        No hay libros en la lista de lectura.
      </div>
      <!-- Hacemos un bucle for para mostrar los libros en la lista de lectura de la misma manera que hicimos en App.vue -->
      <div v-for="libro in listaDeLectura" :key="libro.book.ISBN" class="book-card">
        <img :src="libro.book.cover" :alt="'Portada del libro ' + libro.book.title" class="book-cover" @click="quitarDeLectura(libro)" />
        <h3 class="book-title">{{ libro.book.title }}</h3>
        <p class="book-author">{{ libro.book.author.name }}</p>
      </div>
    </div>
  </aside>
</template>

<script setup>
import { defineProps } from "vue";

defineProps({
  listaDeLectura: Array, //Obtenemos la lista de lectura desde App.vue
  quitarDeLectura: Function //Obtenemos la función quitarDeLectura desde App.vue
});
</script>
