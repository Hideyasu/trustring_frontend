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
        responsive: true,
        maintainAspectRatio: false,
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
