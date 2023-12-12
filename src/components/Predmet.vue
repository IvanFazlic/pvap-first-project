<script setup>
const props = defineProps(["data"])
</script>

<template>
    <tr>
        <td>{{ props.data.idPredmeta }}</td>
        <td>{{ props.data.idProfesora }}</td>
        <td>{{ props.data.naziv }}</td>
        <td>{{ props.data.espb }}</td>
        <td>{{ props.data.status }}</td>
    </tr>
</template>

<style scoped>
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
button:hover{
    cursor: pointer;
}
</style>
