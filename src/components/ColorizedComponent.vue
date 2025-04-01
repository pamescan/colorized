<script setup lang="ts">
const props = defineProps({
  colors: {
    type: Array<string>,
    default: () => [],
  },
  name: {
    type: String,
    default: () => '',
  },
})

function hexToAndroidColor(hex: string) {
  return parseInt(`FF${hex}`, 16) | 0
}
function hexColorsToAndroidColors(colors: string[]) {
  return colors.map(hexToAndroidColor)
}
function generateAndroidColorsFile(colors: string[], name: string) {
  const androidColors = hexColorsToAndroidColors(colors)
  const json = JSON.stringify(
    {
      colors: androidColors,
      name,
    },
    null,
    2,
  )
  return json
}
function generateAndDownloadFile(colors: string[], name: string) {
  // Create file content
  const content = generateAndroidColorsFile(colors, name)

  // Convert string to blob
  const blob = new Blob([content], { type: 'text/plain;charset=utf-8' })

  // Create downloadable link
  const link = document.createElement('a')
  link.href = URL.createObjectURL(blob)
  link.download = `${name}.clrs`

  // Trigger download
  document.body.appendChild(link)
  link.click()

  // Cleanup
  document.body.removeChild(link)
  URL.revokeObjectURL(link.href)
}
</script>
<template>
  {{ name }}
  <button @click="generateAndDownloadFile(props.colors, props.name)">Click</button>
</template>
