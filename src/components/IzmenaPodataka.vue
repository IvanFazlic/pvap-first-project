<script setup>
import {inject} from 'vue'
const student = inject("student")
</script>

<template>
    <h2 v-if="student">{{ student }}</h2>
</template>

<style scoped>
</style>
