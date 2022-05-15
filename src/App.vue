<template>
  <h1>VUE-GOOGLE-BOOKS</h1>

  <input v-model="search"/>
  <button v-on:click="call">Найти</button>
  
  <div class="selectedbook" v-if="item?.volumeInfo?.title">
    <div class="innerSelected"> 
      <button  v-on:click="setItem(null)"> Закрыть</button>
      <img :src="item?.enclosures[0]?.url" />
      <description>
        {{item?.content?.description}}
      </description>
      <h3>{{item?.content?.title}}</h3>
    </div>
  </div>

  <div class="books" v-if="data.length > 0">
    <div v-on:click="setItem(item)" class="book" v-for="item in data" :key="item.id">
      <img :src="item?.enclosures[0]?.url" />
      <h3>{{item?.title}}</h3>  
      <span>{{item?.category}}</span>
    </div>  
  </div>
  <div v-else>
    <h2>Нету новостей(</h2>
  </div>

</template>

<script>

import { parse } from 'rss-to-json'


export default {
  name: 'App',
  data() {
    return {
      loading: true,
      errored: false,
      search: null,
      data: null,
      item: null,
    };
  },
  methods: {
    call:async  function(){
      await parse("https://lenta.ru/rss").then(res => {
      let arr = []
      let length = 0
      res.items.map(y => {
        if (length <= 20) {
          arr.push(y)
          length++
        }
      })
      this.data = arr
    }).finally(() => this.loading = false)
    },
    setItem(item){
      this.item = item
    },
    stop(e){
      e.preventDefault()
    }
  },

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.books{
  width: 90%;
  margin: auto;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: flex-start;
  flex-wrap: wrap;
}
.book{
  width: 290px;
  height: 390px;

  margin: 10px 5px ;

  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 10px;
  border-radius: 15px;
  background: #E3F6FD;
  color: black;
}
.book img{
  height: 150px;
}
.selectedbook{
  width: 100%;
  height: 100vh;

  background: rgba(0,0,0,0.5);

  display: flex;
  align-items:center;
  justify-content:center;

  position: fixed;
  top: 0;
  left: 0;
  z-index: 3,
}
.innerSelected{
  width: 1440px;
  height: 700px;

  background: white;
}
</style>
