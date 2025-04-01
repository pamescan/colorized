<script setup lang="ts">
import { ref } from 'vue'
import ColorizedComponent from './components/ColorizedComponent.vue'
import ColorizedGrid from './components/ColorizedGrid.vue'
// Estado reactivo
const newColor = ref('')
const colors = ref([])
const namePalette = ref('')
// Manejo de eventos
const handleInputColors = (event) => {
  newColor.value = event.target.value
}
const handleInputName = (event) => {
  namePalette.value = event.target.value
}

const addColor = () => {
  if (newColor.value.trim()) {
    const newList: string[] = newColor.value.split(' ')
    newList.forEach((element: string) => {
      colors.value.push(element)
    })
    newColor.value = ''
  }
}
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />
    <!-- Input nativo con reactividad -->
    <input
      type="text"
      :value="newColor"
      @input="handleInputColors"
      placeholder="Ingresa un código de color (ej: FF5733 o #FF5733)"
      @keypress.enter="addColor"
    />
    <br />
    nombre:
    <input
      type="text"
      :value="namePalette"
      @input="handleInputName"
      placeholder="Ingresa un código de color (ej: FF5733 o #FF5733)"
    />
    <button @click="addColor">Agregar Color</button>
    <br />
    <div class="wrapper">
      <ColorizedComponent :name="namePalette" :colors="colors" />
    </div>
  </header>

  <main>
    <ColorizedGrid :initial-colors="colors" />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
