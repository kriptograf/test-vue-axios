<template>
  <div id="app">
    <h2>Bitcoin Price Index</h2>

    <section v-if="errored">
      <p>{{ messageError }}</p>
    </section>

    <section v-else>

      <div v-if="loading">Loading...</div>

      <div v-else v-for="currency in info" :key="currency.id" class="currency">
        <span class="desc">{{ currency.description }}</span>
        <span class="lighten">
          <span v-html="currency.symbol"></span>{{ currency.rate_float | currencydecimal }}
        </span>
      </div>

    </section>
   
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
      info: null, /* Определяем переменную, куда будем складывать ответ сервера */
      loading: true, /* Флаг видимости сообщения о загрузке */
      errored: false, /* Флаг ошибки */
      messageError: null /** Непостредственно сообщение об ошибке */
    };
  },
  filters: {
    currencydecimal (value) {
      return value.toFixed(2)
    }
  },
  mounted(){
    /**
     * Запрашиваем данные с удаленного сервера и присваиваем в переменную info
     * Вызывается сразу после монтирования экземпляра, когда взамен el создан
     */
    axios.get('https://api.coindesk.com/v1/bpi/currentprice.json')
    .then(response => {
      this.info = response.data.bpi;
    })
    .catch(error => {
      this.messageError = error + '. Were sorry, were not able to retrieve this information at the moment, please try back later';
      this.errored = true;/** Обрабатываем и выводим ошибки, если они есть */
    })
    .finally(() => (this.loading = false));/** Устанавливаем флаг видимости сообщения о загрузке в false */
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
