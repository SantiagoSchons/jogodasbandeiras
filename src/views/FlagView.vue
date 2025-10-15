<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { ref } from 'vue';
import  { useFlagStore } from '@/stores/flag';
import { useAppStore } from '@/stores/app';
const flagStore = useFlagStore();
const appStore = useAppStore();
appStore.sortearBandeiras(flagStore.flags);
const audioAcerto = new Audio("/audio/acerto.wav");
const audioErro = new Audio("/audio/erro.wav");




const acertos = ref(0);
appStore.tempoini = Date.now();
const intervalo = setInterval(() => {
  let tempoD = Date.now() - appStore.tempoini
  appStore.cronometro = new Date(tempoD).toString().substring(19,24)
  // if (appStore.cronometro == "00:30"){
  //   appStore.bandeirasSorteadas = []
  //   appStore.sortearBandeiras(flagStore.flags)
  //   appStore.tempoini = Date.now();
  //   alert("O tempo acabou")
  // }
}, 1000);



function verificarAcerto(id){
  console.log(id)
  if(appStore.bandeiraCerta.id == id) {
    appStore.bandeirasSorteadas = []
    appStore.sortearBandeiras(flagStore.flags)
    if (appStore.mudo == false){
      audioAcerto.play();
    }
    acertos.value = acertos.value + 1;
    console.log(acertos)
  }
  if(appStore.bandeiraCerta.id != id) {
    if (appStore.mudo == false){
      audioErro.play()
    }
    
  }
  if(acertos.value == 3){
    alert("fechou")
    appStore.tempoini = Date.now();
    acertos.value = 0;
  }
}

function mutarSom(){
  if (appStore.mudo == false){
    appStore.mudo = true
    document.getElementById("mudo").style.opacity=0.5;
  }else{
    appStore.mudo = false
        document.getElementById("mudo").style.opacity=1;
  }
}


</script>

<template>
<div class="container">
  <header>
    <img :src="'relogio.png'" alt="">
    <p>{{ appStore.cronometro }}</p>
    <img class="botaoMudo" id="mudo" :src="'volume.png'" alt="" @click="mutarSom()">
  </header>

  <section class="bandeiras">
    <h2>{{ appStore.bandeiraCerta.name }}</h2>  
    <ul>
      <li v-for="bandeira in appStore.bandeirasSorteadas">
        <img :src="'flags/' + bandeira.image" alt="" @click="verificarAcerto(bandeira.id)">
      </li>
    </ul>
  </section>

  <section class="contador">
    <p>{{ acertos }} / 3</p>
    <p>acertos</p>
    <p><router-link to="/">Sair</router-link></p>
  </section>
</div>
</template>

<style>

/* .container{
  display: block;
  margin: 0, auto;
  text-align: center;
} */
header{
  display: flex;
  & p{
    margin-top: 1vw;
  }
  & img.botaoMudo{
    margin-left: auto;
    width: 32px;
    height: 32px;
  }
}

section.bandeiras h2{
  background: rgb(77, 160, 77, 0.2);
  text-align: center;
  padding: 20px;
  margin-top: 10vw;
}


section.bandeiras ul {
  display: grid; /* Define o elemento como um grid */
  grid-template-columns: repeat(3, 1fr); /* Cria 3 colunas com larguras iguais */
  grid-template-rows: repeat(3, 1fr);    /* Cria 3 linhas com alturas iguais */
  /* margin: 24px, 32px; */
  list-style: none;
  gap: 3vw;
  padding: 0;
  margin-top: 7vw;
  & li{ 
      background-color: #69f0f0, 0.5;
      background: rgba(0, 255, 255, 0.2);
      border-style: solid;
      border-width: 1px;
      border-color:rgba(0, 255, 255);
      display: flex;
      justify-content: center;
      border-radius: 20px;
      & img {
        width: 72px; /* Faz a imagem preencher a largura da célula */
        height: 72px; /* Faz a imagem preencher a altura da célula */
        padding: 5px;
        
      }
    }
}


</style>
