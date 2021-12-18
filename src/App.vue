<template>
  <div id="app">
    <h1 class="h1">Decimal To IEEE745 and IEEE745 To Decimal Online Converter</h1>
    <BinToIeee/>
    <h6 class="h6">Made By <a href="https://www.linkedin.com/in/zied-maaloul-162a9b96/" target="_blank" class="h6a">Zied Maaloul</a></h6>

  </div>
</template>

<script>
import BinToIeee from './components/BinToIeee'

export default {
  name: 'App',
  components: {
    BinToIeee
  }
}
</script>

<style>
.h6{
  padding-top: 19px;
  font-size: 17px;
  float: right;
  margin-right: 19px;
}
.h6a {
    color: black;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.h1 {
    font-size: xx-large !important;
    margin-bottom: 33px !important;
    font-weight: 899 !important;
    color: brown;
}
</style>
