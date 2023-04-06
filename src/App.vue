<template>
  <div id="app">
    <div class="urna">
      <Tela
        :tela="tela"
        :numeroVoto="numeroVoto"
        :quantidadeNumeros="quantidadeNumeros"
        :candidato="candidato"
        />
        <Teclado
        :adicionarNumero="adicionarNumero"
        :corrigir="corrigir"
        :confirmar="confirmar"
        :votarEmBranco="votarEmBranco"
      />
    </div>
  </div>
</template>

<script>
import '@/css/global.css'
import Tela from '@/components/Tela.vue'
import Teclado from '@/components/Teclado.vue'
import confirmAudio from './assets/audios/confirm.wav'
import keyAudio from './assets/audios/key.wav'
  export default {
    name:'App',
    components:{
      Teclado,
      Tela
    },
    methods:{
      adicionarNumero(numero){
        this.executarSom(keyAudio);
        if(this.numeroVoto.length == this.quantidadeNumeros){
          return false;
        }
        this.numeroVoto += ''+numero;
        this.verificarCandidato();
      },
      verificarCandidato(){
        if(this.numeroVoto.length < this.quantidadeNumeros){
          return false;
        }
        if(this.candidatos[this.tela][this.numeroVoto]){
          this.candidato = this.candidatos[this.tela][this.numeroVoto];
          return true;
        }
        this.candidato = {
          nome:"Voto nulo",
          pais:"Voto nulo",
          imagem:''
        }
      },
      corrigir(){
        this.limpar();
        this.executarSom(keyAudio);
      },
      limpar(){
        this.candidato={}
        this.numeroVoto=''
      },
      confirmar(){
        if(this.numeroVoto.length < this.quantidadeNumeros){
          return false;
        }
        return this.avancarTela();
      },
      avancarTela(){
        this.executarSom(confirmAudio);
         this.tela='fim';

         var instancia = this;
         setTimeout(function(){
           instancia.tela="Surfista";
           return instancia.limpar();
          },3000)
      },
      votarEmBranco(){
        if(this.tela=='fim') return false;
        this.limpar();
        this.avancarTela();
      },
      executarSom(arquivoSom){
        if(arquivoSom){
          let audio = new Audio(arquivoSom);
          audio.play();
        }
      }
    },
    data(){
      return{
        tela:'Surfista',
        numeroVoto:'',
        quantidadeNumeros:2,
        candidato:{},
        candidatos:{      
          "Surfista":{
            "15":{
              "nome": "Italo Ferreira",
              "pais": "Brasil - Baiá Formosa",
              "imagem": "./assets/imagens/italo.jpg"
            },
            "10":{
              "nome": "Gabriel Medina",
              "pais": "Brasil - São Sebastiao ",
              "imagem": "./assets/imagens/png-transparent-gabriel-medina-billabong-pipeline-masters-t-shirt-innersport-tour-of-austria-others-tshirt-microphone-arm.png"
            },
            "09":{
              "nome": "Yago Dora",
              "pais": "Brasil - Florianópolis",
              "imagem": "./assets/imagens/Yago-Dora-MN2S.svg"
            },
            "05":{
              "nome": "Jadson Andre",
              "pais": "Brasil - Natal",
              "imagem": "./assets/imagens/jadsonAndre.jpg"
            },
            "77":{
              "nome": "Filipe Toledo",
              "pais": "Brasil - Ubatuba",
              "imagem": "./assets/imagens/Filipe-Toledo_22Finals_DIZ_6220_Thiago-Diz.jpg"
            }
          }
        }
      }
    }
  }
</script>

<style>

#app{
  background-color: var(--background-color);
  width: 100%;
  height: 100%;
  display:flex;
  justify-content: center;
  align-items: center;
}
.urna{
  width: 1000px;
  height: 500px;
  background-color: var(--ballot-box-background-color);
  padding: 30px;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
}
</style>