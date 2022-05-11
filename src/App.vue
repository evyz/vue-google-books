<template>
  <h1>VUE-GOOGLE-BOOKS</h1>

  <input v-model="search"/>
  <button v-on:click="call">Найти</button>
  
  <div class="selectedbook" v-if="item?.volumeInfo?.title">
    <div class="innerSelected"> 
      <button  v-on:click="setItem(null)"> Закрыть</button>
      <description>
        {{item?.volumeInfo.description}}
      </description>
    <img :src="item?.volumeInfo?.imageLinks?.smallThumbnail " />
      <h3>{{item?.volumeInfo?.title}}</h3>
      <author v-for="category in item?.volumeInfo?.categories" :key="category">{{category}}</author>
      <span v-for="author in item?.volumeInfo?.authors" :key="author">
        <li>{{author}}</li>
      </span>
    </div>
  </div>

  <div class="books" v-if="data.length > 0">
    <div v-on:click="setItem(item)" class="book" v-for="item in data" :key="item.id">
    <img :src="item?.volumeInfo?.imageLinks?.smallThumbnail " />
      <h3>{{item?.volumeInfo?.title}}</h3>
      <author v-for="category in item?.volumeInfo?.categories" :key="category">{{category}}</author>
      <span v-for="author in item?.volumeInfo?.authors" :key="author">
        <li>{{author}}</li>
      </span>
    </div>  
  </div>
  <div v-else>
    <h2>aa</h2>
  </div>

</template>

<script>
import axios from 'axios';
import test from './test.json';

export default {
  name: 'App',
  data() {
    return {
      API_KEY: 'AIzaSyAJodG-YJeqQDql5VG2XW61LhhaKTVlwyU',
      loading: true,
      errored: false,
      search: null,
      data: test.items,
      item: null,
    };
  },
  methods: {
    call: function(){
      // console.log(this.search)
      axios.get(`https://www.googleapis.com/books/v1/volumes?q=${this.search}+inauthor:keyes&key=${this.API_KEY}`).then(data => {
        this.data = data.data.items
      })
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
