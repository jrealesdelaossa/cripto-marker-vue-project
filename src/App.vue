<template>
  <div class="container">
    <div class="row">
      <h1>Crypto Market</h1>

      <input type="text" name="" id="" class="form-control bg-dark text-light rounded-0 border-0 my-4"
        placeholder="Search Coin" @keyup="searchCoin()" v-model="textSearch">

      <table class="table table-dark">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">
              {{ title }}
            </th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="coin, index in filteredCoins" :key="coin.id">
            <td class="text-muted">
              {{ index + 1 }}
            </td>

            <td>
              <img :src="coin.image" alt="cripto image" class="me-2">
              <span>
                {{ coin.name }}
              </span>
              <span class="ms-2 text-uppercase text-muted">
                {{ coin.symbol }}
              </span>
            </td>

            <td>
              $ {{ coin.current_price }}
            </td>

            <td :class="coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger'">
              {{ coin.price_change_percentage_24h }} %
            </td>

            <td>
              $ {{ coin.total_volume.toLocaleString() }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',

  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: [
        "#",
        "Coin",
        "Price",
        "Price Change",
        "24h Volume"
      ],
      textSearch: "",
    }
  },

  // realiza acciones al cargarse el componente
  async mounted() {
    // realiza petición a la api
    const response = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false');
    // convierte los datos en un formato json
    const data = await response.json();
    console.log(data);
    this.coins = data;
    this.filteredCoins = data;
  },

  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  }
}
</script>

<style scoped>
img {
  width: 2rem;
}
</style>
