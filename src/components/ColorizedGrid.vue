<template>
  <div class="container">
    <!-- Cuadrícula de colores -->
    <div class="grid-container">
      <div
        v-for="(color, index) in colors"
        :key="index"
        class="grid-item"
        :style="{ backgroundColor: color }"
      >
        {{ index }}
      </div>
    </div>

    <!-- Controles -->
    <div class="controls">
      <label>
        Número de columnas:
        <input type="number" min="1" max="12" v-model="columns" />
      </label>

      <button @click="addRandomColor">Agregar Color Aleatorio</button>
      <button @click="removeLastColor">Eliminar Último Color</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, defineProps, watchEffect } from 'vue'
const normalizeColor = (color: string): string => {
  // Si el color ya tiene #, lo devolvemos tal cual
  if (color.startsWith('#')) return color

  // Si no tiene # y no es un color nombrado, agregamos el #
  return '#' + color
}
// Definición de las propiedades que aceptará el componente
const props = defineProps({
  initialColors: {
    type: Array<string>,
    default: () => [],
  },
})
// Estado interno para los colores y columnas
const colors = ref(props.initialColors.map(normalizeColor))
const columns = ref(10)
// Función auxiliar para normalizar colores

watchEffect(() => {
  colors.value = props.initialColors.map(normalizeColor)
})

// Función para agregar un color aleatorio
function addRandomColor() {
  colors.value.push(`#${Math.floor(Math.random() * 16777215).toString(16)}`)
}

// Función para eliminar el último color
function removeLastColor() {
  if (colors.value.length > 0) {
    colors.value.pop()
  }
}
</script>

<style scoped>
/* Los estilos permanecen iguales */
.container {
  padding: 20px;
}

.grid-container {
  display: grid;
  gap: 10px;
  margin-bottom: 20px;
}

.grid-container {
  grid-template-columns: repeat(v-bind(columns), 1fr);
}

.grid-item {
  padding: 20px;
  text-align: center;
  color: white;
  border-radius: 4px;
  font-weight: bold;
}

.controls {
  margin-top: 20px;
}

.controls label {
  margin-right: 20px;
}

button {
  padding: 8px 16px;
  margin-left: 10px;
  cursor: pointer;
}

input[type='number'] {
  width: 60px;
  padding: 4px;
}
</style>
