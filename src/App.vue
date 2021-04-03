<template>
  <div id="app">
    <img alt="Cronometro" src="./assets/logo.png">
    <a class="timer">{{numero}}</a>
    <div class="buttons">
      <button class="button" @click="start()">{{button}}</button>
      <button class="button" @click="clear()">LIMPAR</button>
    </div>
    <div class="list" v-show="history.length > 0">
      <ul>
        <li v-for="item in history" :key="item">VOCÊ FEZ UMA PAUSA EM: {{item}}</li>
      </ul>
      <button @click="history = []">Limpar histórico</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data(){
    return{
      numero: '00:00:00',
      button: 'START',
      timer: null,
      ss: 0,
      mm: 0,
      hh: 0,
      history: []
    }
  },
  methods:{
    start(){
      if(this.timer !== null){
        // AQUI TEM ALGO NO TIMER 
        clearInterval(this.timer);
        this.timer = null;
        this.button = 'START';
        this.saveHistory();
      }else{
        this.timer = setInterval(() =>{
          this.runTimer();
        }, 5);
        this.button = 'PAUSE';
      }
    },
    clear(){
      if(this.timer !== null){
        clearInterval(this.timer);
        this.timer = null;
      }
      this.history = [];
      this.button = 'START';
      this.numero = '00:00:00';
      this.ss = 0;
      this.mm = 0;
      this.hh = 0;
    },
    runTimer(){
      // SOMAR UM SEGUNDO
      this.ss++;
      // SE OS SEGUNDOS CHEGAREM EM 59
      if(this.ss == 59){
        this.ss = 0; //ZERAR OS SEGUNDOS
        this.mm++; //SOMAR UM MINUTO 
      }
      // SE OS MINUTOS CHEGAREM EM 59
      if(this.mm == 59){
        this.mm = 0;
        this.hh++;
      }
      // FORMATO DA HORA
      let format = (this.hh < 10 ? '0'+this.hh : this.hh ) + ':';
      format += (this.mm < 10 ? '0'+this.mm : this.mm ) + ':';
      format += (this.ss < 10 ? '0'+this.ss : this.ss );

      return this.numero = format;

    },
    saveHistory(){
      if(this.ss !== null){
        this.history.push(this.numero);
      }
    }
  }
}
</script>

<style>
  #app{
    display: flex;
    width: 100%;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  img{
    width: 390px;
    height: 390px;
    padding-top: 30px;
  }

  .timer{
    color: #fff;
    font-size: 45px;
    margin-top: -192px;
  }

  .buttons{
    margin-top: 155px;
    display: flex;
  }

  .buttons .button{
    width: 150px;
    background: #fff;
    font-size: 20px;
    border: 0px;
    border-radius: 5px;
    text-align: center;
    margin-left: 15px;
    margin-right: 15px;
    padding: 6px;
    cursor: pointer;
    -webkit-user-select: none;
    transition: all .50s;
  }

  .buttons .button:hover{
    opacity: .8;
  }

  ul{
    text-align: center;
    padding: 0px;
  }

  ul li{
    margin-top: 4px;
    padding: 15px;
    background: rgba(70,70,70);
    list-style: none;
    color: #fff;
    font-size: 18px;
    border-radius: 6px;
  }

  .list button{
    cursor: pointer;
    border: 0px;
    background: #fff;
    border-radius: 5px;
    padding: 8px;
    margin-bottom: 12px;
  }

</style>
