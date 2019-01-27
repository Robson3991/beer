<template>
  <div class="wrapper">
    <div class="search">
      <label for="search">Search</label>
      <input name="search" v-model="searchValue">
    </div>
    <table class="beer-list" v-for="post in filteredList">
      <tr v-for="(value, key) in post">
          <td class="beer-ist--name">{{ key }}</td>
          <td class="beer-list--key">{{ value }}</td>
     </tr>
    </table>
  </div>
</template>

<script>
import Axios from 'axios';
const beerApi = 'https://api.myjson.com/bins/1dmbtq';

export default {
  name: 'Beer',
  data() {
    return {
      productList: [],
      searchValue: ''
    }
  },
  async created() {
    let data = (await Axios.get(beerApi)).data;
    this.productList = data;
  },
  computed: {
    filteredList() {
      return this.productList.filter(post => {
        return post.name.toLowerCase().includes(this.searchValue.toLowerCase())
      })
    }
  },
}
</script>

<style scoped lang="scss">
  .beer-list {
    width: 100%;
    margin: 20px 0px;
    tr {
      margin: 0;
      padding: 0;
    }
    td {
      border: 1px solid grey;
      padding: 5px;
    }
  }
    
</style>
