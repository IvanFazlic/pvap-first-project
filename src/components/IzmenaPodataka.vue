<script setup>
import { watch , ref ,toRef, reactive} from 'vue';
import axios from 'axios';
const props = defineProps(["data"])
const emits = defineEmits(["izmenaRefresh"])

let studentZaIzmenu = ref({})
let ime = ref("")
let prezime = ref("")
let smer = ref("")
let broj = ref("")
let identification = ref(-1)

function CleanUp(){
    identification.value = -1
    ime.value = undefined
    prezime.value = undefined
    smer.value = undefined
    broj.value = undefined
}

watch(props,()=>{
    studentZaIzmenu = props.data.values

    identification.value = studentZaIzmenu.idStudenta
    ime.value = studentZaIzmenu.ime
    prezime.value = studentZaIzmenu.prezime
    smer.value = studentZaIzmenu.smer
    broj.value = studentZaIzmenu.broj
})
function izmeniStudenta() {
    studentZaIzmenu.ime = ime.value
    studentZaIzmenu.prezime = prezime.value
    studentZaIzmenu.smer = smer.value
    studentZaIzmenu.broj = broj.value
    if(studentZaIzmenu.id != -1){
        axios.put(`http://pabp.viser.edu.rs:8000/api/Students/${identification.value}`,studentZaIzmenu).then(()=>{
            emits('izmenaRefresh')
        }).catch((err)=>{
            alert(err)
        }).finally(()=>{
            CleanUp()
        })
    }
    CleanUp()
}
</script>

<template>
    <div>
        <h2>Forma sa izmenu studenta</h2>
        <label for="fname">Ime:</label><br>
        <input type="text" id="fname" name="fname" v-model="ime"><br>
        <label for="lname">Prezime:</label><br>
        <input type="text" id="lname" name="lname" v-model="prezime"><br>
        <label for="lsmer">Smer:</label><br>
        <input type="text" id="lsmer" name="lsmer" v-model="smer"><br>
        <label for="lbroj">Broj:</label><br>
        <input type="text" id="lbroj" name="lbroj" v-model="broj"><br><br>
        <input v-if="identification!=-1" type="submit" value="Submit" @click="izmeniStudenta()">
    </div>
</template>

<style scoped>
</style>
