<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm8
      md6
    >
      <p>Data Indonesia</p>
      <div
        v-if="loading"
      >
        <v-skeleton-loader
          ref="skeleton"
          type="list-item"
          tile="false"
          min-width="800px"
          v-for="item in fakeData"
          :key="item"
        />
      </div>
      <v-list-item
        v-for="item in data"
        :key="item.attributes.FID"
        two-line
        v-else
      >
        <v-list-item-content>
          <v-list-item-title>{{ item.attributes.Provinsi }}</v-list-item-title>
          <v-list-item-subtitle>Positif: {{ item.attributes.Kasus_Posi }}</v-list-item-subtitle>
          <v-list-item-subtitle>Sembuh: {{ item.attributes.Kasus_Semb }}</v-list-item-subtitle>
          <v-list-item-subtitle>Meninggal: {{ item.attributes.Kasus_Meni }}</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      data: [],
      dataJambi: [],
      loading: false,
      fakeData: 34
    }
  },
  created () {
    this.fetchData()
  },
  methods: {
    async fetchData () {
      try {
        this.loading = true
        const response = await axios.get('https://api.kawalcorona.com/indonesia/provinsi')
        this.data = response.data
        console.log('data sudah ada')
        this.loading = false
        this.dataJambi = this.filterJambi()
      } catch (error) {
        console.log(error)
      }
    },
    filterJambi () {
      return this.data.filter((atribute) => {
        return atribute.attributes.Provinsi === 'Jambi'
      })
    }
  }
}
</script>

<style>

</style>
