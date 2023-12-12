<script setup>
import { watch, ref } from 'vue';

const props = defineProps(["data", "podaciZapisnika"])
const prosek = ref(0)


watch(props, () => {
  let suma = 0
  let brojPolozenihIspita = 0
  if(props.podaciZapisnika.length > 0){
    props.podaciZapisnika.forEach(element => {
      if(element.idStudenta == props.data.idStudenta && Number(element.ocena) > 5){
        suma += element.ocena
        brojPolozenihIspita += 1
      }
    });
  }
  brojPolozenihIspita ? prosek.value = Number(suma/brojPolozenihIspita).toFixed(2) : prosek.value = "/"
})

</script>

<template>
  <tr>
    <td>{{ data.ime }}</td>
    <td>{{ data.prezime }}</td>
    <td>{{ `${data.smer}-${data.broj}/${data.godinaUpisa}`.replace(' ', '') }}</td>
    <td>{{ prosek }}</td>
    <td><button @click="$emit('handleRequest', data, 'izmena')">Izmena</button></td>
    <td><button @click="$emit('handleRequest', data, 'predmeti')">Prikazi predmete</button></td>
  </tr>
</template>

<style scoped>
table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
}

tr:hover {
  background-color: aqua;
}
</style>
