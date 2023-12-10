<script setup>
import { reactive ,ref, onMounted} from 'vue';
import IzmenaPodataka from './components/IzmenaPodataka.vue';
import Studenti from './components/Studenti.vue';
import axios from 'axios';
defineEmits(["handleRequest"])
let inicijalniStudenti = ref([])
function dohvatiPodatke() {
    axios.get("http://pabp.viser.edu.rs:8000/api/Students").then(res => {
        inicijalniStudenti.value = res.data
    })
}
onMounted(() => dohvatiPodatke())
const studentZaIzmenu = reactive({})
const prikazi = ref(false)

function handleRequest(vrednost, req){
    prikazi.value = true
    if(req == 'izmena'){
        studentZaIzmenu.values = vrednost
    }else if(req == 'predmeti'){
        console.log(req);
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
        </div>
    </div>

    
</template>

<style scoped>
.studenti{
    width: 30vw;
}
</style>
