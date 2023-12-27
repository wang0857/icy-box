<script setup>
  import { ref } from 'vue'
  import CityService from './services/city.service'
  import SearchBar from './components/SearchBar.vue'
  import Results from './components/Results.vue'

  const tabindex = ref(0)
</script>

<template>
  <div class="app-container">
    <SearchBar
      :search="search"
      @onSearchHandling="onSearchHandling"
      @restartSearchHandling="restartSearchHandling"
    />
    <Results
      :search="search"
      :searchedCities="searchedCities"
      :hasSetResult="hasSetResult"
      @onResultHandling="onResultHandling"
    />
  </div>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        search: '',
        dataList: [],
        hasSetResult: false
      }
    },
    created() {
      CityService.get('https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json')
      .then(data => {
        this.dataList = data.data
      })
      .catch(error => console.log(error))
    },
    computed: {
      searchedCities() {
        return this.dataList.filter(
          data => data.city.toLowerCase().includes(
                  this.search.includes(',') ? this.search.split(',')[0].toLowerCase() : this.search.toLowerCase()
                  ) || 
                  data.state.toLowerCase().includes(
                    this.search.includes(',') ? this.search.split(',')[1].toLowerCase() : this.search.toLowerCase()
                  )
        )
      }
    },
    methods: {
      onSearchHandling(search) {
        this.search = search
      },
      onResultHandling(result) {
        this.search = result
        this.hasSetResult = true
      },
      restartSearchHandling(state) {
        this.hasSetResult = state
      }
    },
  }
</script>

<style scoped lang="scss">
  @import './scss/variables';

  .app-container {
    width: 480px;
  }

  @media screen and (max-width: 768px) {
    .app-container {
      width: 308px;
    }
  }
</style>
