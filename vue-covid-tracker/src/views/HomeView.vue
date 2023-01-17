<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataData" />

    <DataBoxes :stats="status" />
  </main>

  <main class="flex flex-col align-center 
  justify-center 
  text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadingImage" alt="" class="w-24 m-auto">
  </main>
</template>

<script>
import DataTitle from '@/components/DataTitle.vue'
import DataBoxes from '@/components/DataBoxes.vue'

export default {
  name: 'HomeView',
  components: {
    DataTitle,
    DataBoxes
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loadingImage: require('../assets/hourglass.gif')
    }
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    }
  },
  async created() {
    const data = this.fetchCovidData()
    
    this.dataDate = data.dataDate
    this.stats = data.GlobalComponents
    this.countries = data.Countries
    this.loading = false
  },
}
</script>
