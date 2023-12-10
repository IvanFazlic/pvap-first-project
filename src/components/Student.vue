<script setup>
const props = defineProps(["data"])
</script>

<template>
    <tr>
        <td>{{ data.ime }}</td>
        <td>{{ data.prezime }}</td>
        <td>{{ `${data.smer}-${data.broj}/${data.godinaUpisa}`.replace(' ','') }}</td>
        <td><button @click="$emit('handleRequest', data, 'izmena')">Izmena</button></td>
        <td><button @click="$emit('handleRequest', data, 'predmeti')">Prikazi predmete</button></td>
    </tr>
</template>

<style scoped>
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
tr:hover{
  cursor: pointer;
  background-color: aqua;
}
</style>
