<script setup>
import { onMounted, reactive, ref, watch, computed, provide } from 'vue';
import Student from './Student.vue';
import axios from 'axios'
const kriterijumPretrage = ref("")
const providedStudent = ref("a")
provide("student", providedStudent)
let inicijalniStudenti = ref([])

onMounted(() => axios.get("http://pabp.viser.edu.rs:8000/api/Students").then(res => {
    inicijalniStudenti.value = res.data
}))

const studenti = computed(()=>inicijalniStudenti.value.filter(student=>{
    for(let studentKey in student){
        if(student[studentKey].toString().toLowerCase().includes(String(kriterijumPretrage.value.toLowerCase())))return 1
    }
}))

</script>

<template>
    Pretraga: <input type="text" v-model="kriterijumPretrage">
    <br><br>
    <table>
        <thead>
            <th>Ime</th>
            <th>Prezime</th>
            <th>Broj indeksa</th>
        </thead>
        <tbody>
            <Student v-for="podatak in studenti" :data="podatak" @click="console.log(podatak)" />
        </tbody>
    </table>
</template>

<style scoped>
table,
th,
td {
    border: 1px solid black;
    border-collapse: collapse;
}</style>
