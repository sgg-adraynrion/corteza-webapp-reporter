<template>
  <canvas
    ref="chart"
    class="h-100 w-100"
  />
</template>
<script>
import base from './base'
import Chart from 'chart.js'
import Funnel from 'chartjs-plugin-funnel'
import colorschemes from 'chartjs-plugin-colorschemes'

export default {
  extends: base,

  data () {
    return {
      chart: undefined,

      plugins: [
        Funnel,
        colorschemes,
      ],
    }
  },

  computed: {
    localDataframe () {
      return this.dataframes[0]
    },
  },

  watch: {
    dataframes: {
      immediate: true,
      deep: true,
      handler (dataframes = []) {
        if (dataframes.length) {
          this.$nextTick(() => {
            this.renderChart()
          })
        }
      },
    },
  },

  methods: {
    renderChart () {
      if (this.chart) {
        this.chart.destroy()
      }

      const ctx = this.$refs.chart.getContext('2d')

      const chartConfig = this.options.getChartConfiguration(this.dataframes)

      this.chart = new Chart(ctx, { ...chartConfig, plugins: this.plugins })
    },
  },
}
</script>
