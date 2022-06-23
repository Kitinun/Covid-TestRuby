<template>
    <div>
        <div class="px-2">
            <h1>Graph Covid</h1>
            <b-button variant="info" href="/">Go to view charts</b-button>
        </div>
  </div>
</template>

<script>
export default {
    name: 'GraphPage',
    data() {
        return {
            graphCovid: [],
        }
    },
    async fetch() { 
        this.covidData = await this.$axios.$get('https://disease.sh/v3/covid-19/historical?lastday=30');
    },
    methods: {
    }
}
</script>