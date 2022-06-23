<template>
  <div>
    <div class="px-2">
      <h1>Total Covid</h1>
      <b-button variant="info" href="/charts">Go to view Graph</b-button>
      <div class="mt-2">
        <b-datepicker v-model="dateToday" @input="getCovidD"></b-datepicker>
      </div>
      <div class="mt-2">
        <b-table striped hover :items="this.covidPickedData"></b-table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      covidData: [],
      covidPickedData: [],
      dateToday: new Date().toString(),
    }
  },
  async fetch() { 
    this.covidData = await this.$axios.$get(
      'https://disease.sh/v3/covid-19/historical?lastday=30'
    );
    
    const dateFormatJHUCSSE = new Date()
    const format = `${
      dateFormatJHUCSSE.getMonth() + 1
    }/${dateFormatJHUCSSE.getDate()}/${dateFormatJHUCSSE.getFullYear() % 100}`

    this.covidPickedData = this.covidData.map(d => {
        return { 
          country: d.country,
          province: d.province ?? '-',
          case: d.timeline.cases[format],
          deaths: d.timeline.deaths[format],
          recovered: d.timeline.recovered[format]
        }
      })
  },
  methods: {
    getCovidD() {
      const dateFormatJHUCSSE = new Date(this.dateToday)

      const format = `${
        dateFormatJHUCSSE.getMonth() + 1
      }/${dateFormatJHUCSSE.getDate()}/${dateFormatJHUCSSE.getFullYear() % 100}`

      this.covidPickedData = this.covidData.map(d => {
        return { 
          country: d.country,
          province: d.province ?? '-',
          case: d.timeline.cases[format],
          deaths: d.timeline.deaths[format],
          recovered: d.timeline.recovered[format]
        }
      })
    },
  }
}
</script>