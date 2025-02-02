<template>
  <div></div>
</template>

<script setup>
import { ref, watch, onMounted, onUnmounted, defineEmits, defineExpose } from "vue";

const emit = defineEmits(["actualizarLista"]);
const listaDeLectura = ref(JSON.parse(localStorage.getItem("listaDeLectura") || "[]"));

// 🔹 Sincroniza `listaDeLectura` entre pestañas
const sincronizarLista = (event) => {
  if (event.key === "listaDeLectura") {
    listaDeLectura.value = JSON.parse(event.newValue || "[]");
    emit("actualizarLista", listaDeLectura.value);
  }
};

// 🔹 Escuchar y dejar de escuchar eventos de `storage`
onMounted(() => window.addEventListener("storage", sincronizarLista));
onUnmounted(() => window.removeEventListener("storage", sincronizarLista));

// 🔹 Guardar en `localStorage` cuando cambie `listaDeLectura`
watch(listaDeLectura, (nuevaLista) => {
  localStorage.setItem("listaDeLectura", JSON.stringify(nuevaLista));
  emit("actualizarLista", nuevaLista);
}, { deep: true });

// 🔹 Exponer `actualizarLista` para que `App.vue` lo use
defineExpose({ actualizarLista: (nuevaLista) => listaDeLectura.value = nuevaLista });
</script>


