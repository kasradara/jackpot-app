<template>
  <div class="container">
    <span class="credit">credit : {{credit}}</span>
    <table class="symbols">
        <td v-for="x in 3" :key="x" :class="['symbols-placeholder', {left33:x==2, left66:x==3}]">
          <span :class="['symbol-charecter', {loading:loading}]">
            x
          </span>
        </td>
        <td class="symbol" v-for="(symbol, index) in result" :key="Math.random() * index">
          <span class="symbol-charecter">{{symbol}}</span>

        </td>
    </table>
    <div class="buttons" v-if="credit">
      <button class="btn" :disabled="disableSpin || cashedOut" @click="getResult">spin</button>
      <button
        :class="['btn', {cashout: moveCashout}]" 
        :disabled="disableCashout || cashedOut"
        @mouseenter="cashOutHover()"
        @click="cashedOut = true"
      >
        cash out
      </button>
    </div>
    <p class="message">{{message}}</p>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      credit: 10,
      symbols: [
        {name: 'cherry', reward: 10, slug: 'C'},
        {name: 'lemon', reward: 20, slug: 'L'},
        {name: 'orange', reward: 30, slug: 'O'},
        {name: 'watermelon', reward: 40, slug: 'W'}
      ],
      result: [],
      loading: false,
      winsOnce: false,
      disableSpin: false,
      disableCashout: false,
      moveCashout: false,
      cashedOut: false,
    }
  },
  computed: {
    message() {
      if(!this.credit){
        return 'you have no credit to spin or cashout';
      }
      if(this.cashedOut){
        return `you cashedout ${this.credit} credit`;
      } 
      if(this.disableCashout || this.moveCashout) {
        return 'you cant cashout right now'
      } 
      return ''

      
    }
  },
  methods: {
    async spin() {
      let symbols = []
      for (let i = 0; i < 3; i++) {
        const symbolRandomId = Math.floor(Math.random()*4);
        symbols.push(this.symbols[symbolRandomId].slug)
      }
      return await symbols
    },
    getResult() {
      this.loading = true
      this.disableSpin = true
      this.result = []
      this.spin().then(res => {
        if(res.every(symbol => symbol == res[0]) && !this.winsOnce && this.couldCheat()){
          this.winsOnce = true;
          this.getResult()
          return 0
        }
        this.result = []
        res.forEach((element,ind) => {
          setTimeout(()=>{
            this.result.push(element)
          }, (1+ind)*1000)
        });
        setTimeout(()=>{
          this.setCredit(res)
          this.winsOnce = false
          this.disableCashout = false
          this.disableSpin = false
          this.moveCashout = false
        }, 3000)
      })
    },
    couldCheat(){
      let percent = Math.floor(Math.random()*101)
      if(this.credit >= 40 && this.credit < 60 && percent <= 30) {
        return true
      }
      if(this.credit >= 60 && percent <= 60) {
        return true
      } return false
    },
    setCredit(symbols) {
      if(symbols.every(symbol => symbol == 'C')){
        this.credit = this.credit + 10
      } else if(symbols.every(symbol => symbol == 'L')){
        this.credit = this.credit + 20
      } else if(symbols.every(symbol => symbol == 'O')){
        this.credit = this.credit + 30
      } else if(symbols.every(symbol => symbol == 'W')){
        this.credit = this.credit + 40
      } else this.credit--
    },
    cashOutHover() {
      let percent = Math.floor(Math.random()*101)
      if(percent <= 40){
        this.disableCashout = true
      } else if(percent > 40 && percent <= 90 ) {
        this.moveCashout = true
      }
    },
  }
}
</script>

<style>
body{
  margin: 0;
  background-color:antiquewhite
}
.container{
  max-width: 500px;
  height: 80vh;
  border-radius: 20px;
  margin: 20px auto 0;
  background: brown;
}
.credit{
    text-align: center;
    display: block;
    padding-top: 18px;
    font-size: 20px;
    font-weight: 900;
    color: white;
}
.symbols{
  background-color: white;
  margin-top: 32px;
  height: 150px;
  position: relative;
  display: flex;
}
.symbols .symbol{
  font-size: 20px;
  z-index: 1;
  text-align: center;
  background: white;
  width: 33%;
}
.symbol-charecter{
  font-size: 96px;
  text-align: center;
  display: block;
}
.symbols-placeholder{
  width: 33.3%;
  background: white;
  position: absolute;
  height: 148px;
}
.left33{
  left: 33.3%;
}
.left66{
  left: 66.3%;
}
.loading{
  animation-name: spin;
  animation-duration: .5s;
  animation-iteration-count: infinite;
}
.btn{
  width: 50%;
  height: 40px;
}
.cashout{
  position: relative;
  bottom: 300px;
}
.message{
  text-align: center;
  font-size: 18px;
  font-weight: 700;
  color: white;
}
@keyframes spin {
  0% {transform: rotateY(0deg); }
  100% {transform: rotateY(360deg);}
}
</style>
