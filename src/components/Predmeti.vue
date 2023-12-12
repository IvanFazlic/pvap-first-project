<script setup>
import { ref, watch } from 'vue'
import axios from 'axios';
import Predmet from './Predmet.vue'

const toggle = ref(true)
const props = defineProps(["data"])

let sviPredmeti = ref([])
let predmetiStudenta = ref([])
let ispitiStudenta = ref([])
let zapisnikStudenta = ref([])

let predmeti = ref([])
let predmetiZapisnika = ref([])
let test = ref([])

const dohvatiPredmeteStudenta = async () => {
    const dohvatiFiltriraneStudente = await axios.get(`http://pabp.viser.edu.rs:8000/api/StudentPredmets`)
    const dohvatiPredmete = await axios.get(`http://pabp.viser.edu.rs:8000/api/Predmets`)
    const dohvatiZapisnik = await axios.get(`http://pabp.viser.edu.rs:8000/api/Zapisniks`)
    const dohvatiIspite = await axios.get(`http://pabp.viser.edu.rs:8000/api/Ispits`)

    predmetiStudenta.value = await dohvatiFiltriraneStudente.data.filter(predmet => predmet.idStudenta == props.data.values.idStudenta)
    zapisnikStudenta.value = await dohvatiZapisnik.data.filter(zapisnik => zapisnik.idStudenta == props.data.values.idStudenta)
    ispitiStudenta.value = await dohvatiIspite.data
    sviPredmeti.value = await dohvatiPredmete.data

    predmeti.value = sviPredmeti.value.filter(p1 =>
        predmetiStudenta.value.some(p2 => p2.idPredmeta === p1.idPredmeta)
    );

    predmetiZapisnika.value = zapisnikStudenta.value.map(obj1 => {
        const matchingObj = ispitiStudenta.value.find(obj2 => obj1.idIspita === obj2.idIspita);
        return { ...obj1, ...matchingObj }
    });

    predmeti.value = predmeti.value.map(obj1 => {
        const matchingObj = predmetiZapisnika.value.find(obj2 => obj1.idPredmeta === obj2.idPredmeta);
        return { ...obj1, ...matchingObj }
    })
    console.log(predmeti.value);

}
const obrisiPredmet = (idStudenta, idPredmeta) => {
    axios.delete(`http://pabp.viser.edu.rs:8000/api/StudentPredmets/${idStudenta}/${idPredmeta}`)
        .then((res) => {
            dohvatiPredmeteStudenta()
            alert("Uspesno obrisan predmet")
        })
}

const handlePredmet = (arg, req) => {
    if (req == 'brisanjePredmeta') {
        obrisiPredmet(props.data.values.idStudenta, arg.idPredmeta)
    }
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
                <Predmet v-for="p in predmeti" :data="p" @handlePredmet="(arg, req) => handlePredmet(arg, req)"
                    :toggle="toggle"></Predmet>
            </tbody>
        </table>
        <br>
        <button>Dodaj</button>
        <button @click="toggle = !toggle">Prikazi zapisnik</button>
    </div>
    <div>

    </div>
</template>

<style scoped>
table,
th,
td {
    border: 1px solid black;
    border-collapse: collapse;
}

table {
    text-align: center;
}

th {
    padding: 0 5px;
}

button {
    margin-right: 10px;
}
</style>