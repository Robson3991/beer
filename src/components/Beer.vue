<template>
  <div class="wrapper">
    <h1 class="title">Wyszukiwarka browarków :)</h1>
    <div class="search">
      <label for="search">Nazwa </label>
      <input name="search" v-model="searchValue">
      <span> Sortuj po: </span>
      <select v-model="sort">
        <option v-for="item in options" :label="item.label" :value="item.value"></option>
      </select>
    </div>
    <div class="filter-list">
      <div class="filter-option">
          <label>alk</label>
          <input type="number" v-model="filterList.alk">
      </div>
      <div class="filter-option">
        <label>plato</label>
        <input type="number" v-model="filterList.plato">
      </div>
    </div>
    <div class="beer-list">
      <div class="beer-list--item" v-for="post in filteredList">
        <div class="item-parameter" v-for="(value, key) in post">
            <p class="parameter item-parameter__name">{{ key }}</p>
            <p class="parameter item-parameter__key">{{ value }}</p>
        </div>
      </div>
    </div>
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
      searchValue: '',
      sort: 'default',
      options: [
        {label: 'domyślnie', value: 'default'},
        {label: 'stand', value: 'stand'},
        {label: 'alk', value: 'alk'},
        {label: 'plato', value: 'plato'},
        {label: 'ibu', value: 'ibu'},
      ],
      filterList: [
        {alk: ''},
        {plato: ''}
      ]
    }
  },

  async created() {
    let data = (await Axios.get(beerApi)).data;
    this.productList = data;
  },

  computed: {

    filteredList() {
      let beers = this.productList.filter(post => {
        return post.name.toLowerCase().includes(this.searchValue.toLowerCase())
      });

      let sortOption = this.sort;

      function compare(a, b){
        switch(sortOption){
          case 'stand':
            return b.stand - a.stand;
          case 'alk':
            return b.alk - a.alk;
          case 'plato':
            return b.plato - a.plato;
          case 'ibu':
            return b.ibu - a.ibu;
          default:
            return b - a;
        }
      }


      let filters = {};

      if(parseInt(this.filterList.alk) > 0){
        filters.alk = [parseInt(this.filterList.alk)];
      }
      if(parseInt(this.filterList.plato) > 0){
        filters.plato = [parseInt(this.filterList.plato)];
      }

      function multiFilter(array, filters) {

        const filterKeys = Object.keys(filters);
        return array.filter((item) => {
          return filterKeys.every(key => {
            if (!filters[key].length) return true;
            return filters[key].includes(item[key]);
          });
        });
      }

      return multiFilter(beers.sort(compare), filters);
    
    },
  },
}
</script>

<style scoped lang="scss">

  .title {
    margin: 40px 0px;
    font-size: 20px;
    font-weight: 200;
  }
  .beer-list {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    font-size: 13px;
    margin-top: 20px;
    &--item {
      display: flex;
      flex-direction: column;
      width: 30%;
      border: 2px solid #777777;
      margin-bottom: 25px;
    }
  }
  .item-parameter {
    display: flex;
    &__name { width: 30%; }
    &__key { width: 70%; }
    &:nth-child(3){
      flex-grow: 1;
    }
  }
  .parameter {
    margin: 0;
    padding: 5px;
    border: 1px solid #ccc;
  }
  .filter-list {
    display: flex;
    margin-top: 10px;
    justify-content: center;
    .filter-option {
      margin-right: 5px;
    }
  }

</style>
