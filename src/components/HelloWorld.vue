<script setup>
import { ref } from 'vue'
const name = ref('')
</script>
<template>
    <input type="text" v-model="name" />
    <h3>Hello, {{ name }}</h3>
</template>
<style scoped>
h3 {
    color: green
}
</style>