<script>
import { Pie } from 'vue-chartjs'

export default {
  extends: Pie,
  props: {
    result: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      options: {
        tooltips: {
          enabled: true,
          callbacks: {
            label(tooltipItems, data) {
              const label = data.datasets[0].data
              const total = label.reduce(function (sum, element) {
                return sum + element
              }, 0)
              const labelText = `${
                data.labels[tooltipItems.index]
              }:${Math.round((label[tooltipItems.index] / total) * 100)}%`
              return labelText
            },
          },
        },
        responsive: true,
        maintainAspectRatio: false,
        legend: {
          labels: {
            fontColor: '#fff',
          },
        },
      },
    }
  },
  watch: {
    result(newData, oldData) {
      this.renderChart(this.result, this.options)
    },
  },
  ready() {
    this.$nextTick(function () {
      this.renderChart(this.result, this.options)
    })
  },
  mounted() {
    this.renderChart(this.result, this.options)
  },
}
</script>
