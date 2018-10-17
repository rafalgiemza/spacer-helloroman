<template>
  <div class="searchWrapper">
    <input
      id="search"
      name="search"
      v-model="searchValue"
      @input="handleInput"
    />
    <ul>
      <li v-for="item in results" :key="item.data[0].nasa_id">
        <p> {{ item.data[0].description }} </p>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';

const API_URL = 'https://images-api.nasa.gov/search';

export default {
  name: 'Search',
  data() {
    return {
      searchValue: '',
      results: [],
    };
  },
  methods: {
    // eslint-disable-next-line
    handleInput: debounce ( function () {
      axios.get(`${API_URL}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
        });
    }, 500),
  },
};
</script>

<style lang='scss' scoped>
  .searchWrpper {
    display: flex;
  }
</style>
