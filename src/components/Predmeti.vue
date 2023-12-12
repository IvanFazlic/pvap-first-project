<script setup>
import { ref, watch } from 'vue'
import axios from 'axios';
import Predmet from './Predmet.vue'

const props = defineProps(["data"])

let sviPredmeti = ref([])
let predmetiStudenta = ref([])
let predmeti = ref([])

const dohvatiPredmeteStudenta = async () => {
    const dohvatiFiltriraneStudente = await axios.get(`http://pabp.viser.edu.rs:8000/api/StudentPredmets`)
    const dohvatiPredmete = await axios.get(`http://pabp.viser.edu.rs:8000/api/Predmets`)

    predmetiStudenta.value = await dohvatiFiltriraneStudente.data.filter(predmet => predmet.idStudenta == props.data.values.idStudenta)
    sviPredmeti.value = await dohvatiPredmete.data

    predmeti.value = sviPredmeti.value.filter(p1 =>
        predmetiStudenta.value.some(p2 => p2.idPredmeta === p1.idPredmeta)
    );
}

watch(props, () => {
    dohvatiPredmeteStudenta()
})
</script>

<template>
    <div v-if="props.data.values">
        <h2>Predmeti studenta {{ props.data.values.ime }} {{ props.data.values.prezime }}</h2>
        <table>
        <thead>
            <th>ID predmeta</th>
            <th>ID profesora</th>
            <th>Naziv predmeta</th>
            <th>Broj ESPB</th>
            <th>Status predmeta</th>
        </thead>
        <tbody>
            <Predmet v-for="p in predmeti" :data="p"></Predmet>
        </tbody>
    </table>
    </div>
</template>

<style scoped>
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
table{
    text-align: center;
}
th{
    padding: 0 5px;
}
</style>