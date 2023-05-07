<template>
  <main v-if="!loading">
  <DataTitle :text="title" :dataDate="dataDate" />
  <dataBoxes :stats="stats" />
  <regionSelect @get-region="getRegionData" :region="region" />

  <button @click="clearRegion"  v-if="stats.loc" class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-non hover:bg-green-900">Clear Region</button>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else="">
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching data
    </div>
    <img class="w-24 m-auto" :src="loadingImage" alt="loading">
  </main>
</template>

<script>
import DataTitle from '@/components/DataTitle.vue'
import dataBoxes from '@/components/dataBoxes.vue'
import regionSelect from '@/components/regionSelect.vue'
export default {
  name: 'HomeView',
  components: {
    DataTitle,
    dataBoxes,
    regionSelect,
  },
  data() {
    return {
      loading: true,
      title: 'India',
      dataDate: '',
      stats: '',
      region: [],
      loadingImage: require('../assets/hourglass.gif')
    }
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch('https://api.rootnet.in/covid19-in/stats/latest',);
      const data = await res.json();
      return data;
    },
    getRegionData(reg){
      console.log(reg);
      this.title=reg.loc;
      this.stats=reg;
    },
    async clearRegion(){
      window.location.reload();

    }
  },
  async created() {
    const data = await this.fetchCovidData();
    this.dataDate = data.lastOriginUpdate;
    this.stats = data.data.summary;
    this.region = data.data.regional;
    this.loading = false;
  }
}
</script>
