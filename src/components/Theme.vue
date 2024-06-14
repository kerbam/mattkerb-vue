<script setup>
import { useDark, useToggle } from '@vueuse/core'

const isDark = useDark();
const toggleDark = useToggle(isDark);
</script>

<template>

    <p>isDark: {{ isDark }}</p>
    <button @click="toggleDark()">Toggle theme</button>
</template>

<style scoped>
button {
    background: var(--ds-border-brand);
    color: var(--ds-text);
}
</style>
