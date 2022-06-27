<template>
  <div class="container">
    <h1 class="text-center text-primary mt-5 fw-bold display-3">Coins</h1>
    <input type="text" placeholder="search" @keyup="searchCoin" v-model="textSearch" class="form-control border border-primary text-primary fw-bold my-3">
    <table class="table table-striped table-bordered">
      <thead class="bg-primary text-light fw-bold">
        <tr>
          <th v-for="title in titles" :key="title">{{title}}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(coin,index) in filteredCoins" :key="coin.id">
          <td>{{index+1}}</td>
          <td>
            <img :src="coin.image" alt="{{coin.name}}" width="20" class="me-2" />
            <span>{{coin.name}}</span>
            <span class="ms-2 text-primary">{{coin.symbol.toUpperCase()}}</span>
          </td>
          <td>${{coin.current_price}} USD</td>
          <td :class="[coin.price_change_percentage_24h>0?'text-success':'text-danger']">{{coin.price_change_percentage_24h}}%</td>
          <td>${{coin.total_volume.toLocaleString()}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>

export default {

  name: 'App',
  data(){
    return {
      coins: [],
      filteredCoins:[],
      titles:[
        '#',
        'Name',
        'Price',
        'Price Change',
        '24h Volume'
      ],
      textSearch:""
    }
  },
  async mounted(){
    const res=await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
    const data= await res.json();
    this.coins=data;
    this.filteredCoins=data;
  },
  methods:{
    searchCoin(){
      this.filteredCoins=this.coins.filter(coin=>
        coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) || 
        coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    }
  }
};
</script>

<style>
</style>
