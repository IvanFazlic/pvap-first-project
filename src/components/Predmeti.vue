<script setup>
import {reactive, ref, watch} from 'vue'
import axios from 'axios';
const props = defineProps(["data"])
let sviPredmeti = ref([])
let predmetiStudenta = ref([])

const dohvatiPredmeteStudenta =async ()=>{
    const filtriraniStudenti = await axios.get(`http://pabp.viser.edu.rs:8000/api/StudentPredmets`)
    predmetiStudenta.value = await filtriraniStudenti.data.filter(predmet=>predmet.idStudenta == props.data.values.idStudenta)
    const sviPredmeti = await axios.get(`http://pabp.viser.edu.rs:8000/api/Predmets`)
    sviPredmeti.value = await sviPredmeti.data
    console.log(sviPredmeti.value);
    console.log(predmetiStudenta.value);
}


watch(props,()=>{
    dohvatiPredmeteStudenta()
})
</script>

<template>
    <div>
        <h2>Predmeti</h2>
    </div>
</template>

<style scoped>

</style>
