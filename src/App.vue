<template>
  <div>
    <span>credit : {{credit}}</span>
    <div class="symbols">
      <symbols v-for="(item) in result" :key="item" :symbol="item" />
    </div>
    <button @click="getResult">spin</button>
  </div>
</template>

<script>
import symbols from '@/components/symbols.vue'

export default {
  components: {symbols},
  name: 'App',
  data() {
    return {
      credit: 10,
      symbols: [
        {name: 'cherry', reward: 10, slug: 'c'},
        {name: 'lemon', reward: 20, slug: 'l'},
        {name: 'orange', reward: 30, slug: 'o'},
        {name: 'watermelon', reward: 40, slug: 'w'}
      ],
      result: ['x','y','z'],
      loading : [false , false , false]
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
        // if(res.every(symbol => symbol == res[0]) ){
        //   console.log('hi')
        // }
        this.result[0] = res[0]
        this.result[1] = res[1]
        this.result[2] = res[2]
        this.loading = true
        console.log(this.result)
      })
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
