<script setup>
import { reactive, ref, watch } from 'vue'
import axios from 'axios';
import Predmet from './Predmet.vue'
const props = defineProps(["data"])
let sviPredmeti = ref([])
let predmetiStudenta = ref([])
let predmeti = ref([])

const dohvatiPredmeteStudenta = async () => {
    const filtriraniStudenti = await axios.get(`http://pabp.viser.edu.rs:8000/api/StudentPredmets`)
    predmetiStudenta.value = await filtriraniStudenti.data.filter(predmet => predmet.idStudenta == props.data.values.idStudenta)
    const sviPredmeti = await axios.get(`http://pabp.viser.edu.rs:8000/api/Predmets`)
    sviPredmeti.value = await sviPredmeti.data

    predmeti.value = sviPredmeti.value.filter(objekat2 =>
        predmetiStudenta.value.some(objekat1 => objekat1.idPredmeta === objekat2.idPredmeta)
    );
}
watch(props, () => {
    dohvatiPredmeteStudenta()
})
</script>

<template>
    <div v-if="predmetiStudenta.length">
        <h2>Predmeti</h2>
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
table{
    text-align: center;
}
th{
    padding: 0 5px;
}
</style>
