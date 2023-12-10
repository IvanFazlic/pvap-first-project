<script setup>
import { reactive ,ref, onMounted} from 'vue';
import IzmenaPodataka from './components/IzmenaPodataka.vue';
import Studenti from './components/Studenti.vue';
import axios from 'axios';
import Predmeti from './components/Predmeti.vue';
defineEmits(["handleRequest"])

let inicijalniStudenti = ref([])
const studentZaIzmenu = reactive({})
const prikazPredmeta = reactive({})

const dohvatiPodatke = ()=>{
    axios.get("http://pabp.viser.edu.rs:8000/api/Students").then(res => inicijalniStudenti.value = res.data)
}
onMounted(() => dohvatiPodatke())


const handleRequest = (vrednost, req)=>{
    if(req == 'izmena'){
        studentZaIzmenu.values = vrednost
    }else if(req == 'predmeti'){
        prikazPredmeta.values = vrednost
    }
}
</script>

<template>
    <div style="display: flex;">
        <div class="studenti">
            <Studenti :inicijalniStudenti="inicijalniStudenti" @handleRequest = '(arg, req) => handleRequest(arg, req)' ></Studenti>
        </div>
        <div>
            <IzmenaPodataka :data=studentZaIzmenu @izmenaRefresh="() => dohvatiPodatke()"/>
            <Predmeti :data=prikazPredmeta></Predmeti>
        </div>
    </div>

    
</template>

<style scoped>
.studenti{
    width: 30vw;
}
</style>
