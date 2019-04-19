<template>
  <div id="app">
    <h2>Bitcoin Price Index</h2>
    <div v-for="currency in info" :key="currency.id" class="currency">
        <span class="desc">{{ currency.description }}</span>
        <span class="lighten">
          <span v-html="currency.symbol"></span>{{ currency.rate_float | currencydecimal }}
        </span>
    </div>
  </div>
</template>

<script>
/**
 * Импортируем библиотеку axios для получения данных с удаленного сервера
 */
import axios from 'axios'

export default {
  name: 'app',
  data(){
    return {
      info: null /* Определяем переменную, куда будем складывать ответ сервера */
    };
  },
  mounted(){
    /**
     * Запрашиваем данные с удаленного сервера и присваиваем в переменную info
     * Вызывается сразу после монтирования экземпляра, когда взамен el создан
     */
    axios.get('https://api.coindesk.com/v1/bpi/currentprice.json')
    .then(response => (this.info = response.data.bpi));
  },
  filters: {
    currencydecimal (value) {
      return value.toFixed(2)
    }
  }
}
</script>

<style>
body{
  padding-top: 60px;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  margin: 0 auto;
  width: auto;
  padding: 30px;
  background: #2F242C;
  color: #B3BFB8;
  width: 400px;
  border-radius: 10px;
}
h2{
  text-align: center;
}
.lighten {
    color: white;
}
.desc{
  width: 250px;
  display: inline-block;
}
</style>
