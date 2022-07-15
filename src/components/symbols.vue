<template>
        <div class="symbol">
            <span>{{symbol}}</span>
        </div>
</template>

<script>


export default {
  name: 'jackPotSymbols',
  props: {
    symbol: Array
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
