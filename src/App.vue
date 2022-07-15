<template>
  <div>
    <span>credit : {{credit}}</span>
    <div class="symbols">
        <div class="symbol" v-for="(symbol, index) in result" :key="index">
            <span>{{symbol}}</span>
        </div>
    </div>
    <button @click="getResult">spin</button>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      credit: 75,
      symbols: [
        {name: 'cherry', reward: 10, slug: 'c'},
        {name: 'lemon', reward: 20, slug: 'l'},
        {name: 'orange', reward: 30, slug: 'o'},
        {name: 'watermelon', reward: 40, slug: 'w'}
      ],
      result: ['','',''],
      winsOnce: false
    }
  },
  created() {
    console.log(Math.floor(Math.random()*4))
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
      this.spin().then(res => {
        console.log(res)
        if(res.every(symbol => symbol == res[0]) && !this.winsOnce && this.couldCheat()){
          this.winsOnce = true;
          this.getResult()
          return 0
        }
        this.result = []
        res.forEach((element,ind) => {
          setTimeout(()=>{
            this.result.push(element)
          }, (3+ind)*1000)
        });

        console.log(this.result)
        this.winsOnce = false
      })
    },
    couldCheat(){
      let percent = Math.floor(Math.random()*101)
      console.log(percent)
      if(this.credit >= 40 && this.credit < 60 && percent <= 30) {
        return true
      }
      if(this.credit >= 60 && percent <= 60) {
        return true
      } return false
    }
  }
}
</script>

<style>
.symbols{
  display: flex;
  gap: 20px;
}
.symbols .symbol{
  font-size: 20px;
  text-align: center;
  background: gray;
  width: 33%;
}
</style>
