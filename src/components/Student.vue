<script setup>
defineProps(["data"])
</script>

<template>
    <tr>
        <td>{{ data.ime }}</td>
        <td>{{ data.prezime }}</td>
        <td>{{ `${data.smer}-${data.broj}/${data.godinaUpisa}`.replace(' ','') }}</td>
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
