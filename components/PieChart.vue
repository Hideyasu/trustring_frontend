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
    this.addPlugin({
      afterDraw(chart, go) {
        const ctx = chart.ctx
        chart.data.datasets.forEach((dataset, i) => {
          let dataSum = 0
          dataset.data.forEach((element) => {
            dataSum += element
          })

          const meta = chart.getDatasetMeta(i)
          if (!meta.hidden) {
            meta.data.forEach(function (element, index) {
              if (dataset.data[index] / dataSum === 0) {
                return
              }
              // フォントの設定
              const fontSize = 14
              const fontStyle = 'normal'
              const fontFamily = 'Helvetica Neue'
              ctx.fillStyle = '#333'
              // 設定を適用
              ctx.font = Chart.helpers.fontString( // eslint-disable-line
                fontSize,
                fontStyle,
                fontFamily
              )

              // ラベルをパーセント表示に変更
              const labelString = chart.data.labels[index]
              const dataString =
                Math.round((dataset.data[index] / dataSum) * 100).toString() +
                '%'

              // positionの設定
              ctx.textAlign = 'center'
              ctx.textBaseline = 'middle'

              const padding = -2
              const position = element.tooltipPosition()
              // ツールチップに変更内容を表示
              ctx.fillText(
                labelString,
                position.x,
                position.y - fontSize / 2 - padding
              ) // title
              ctx.fillText(
                dataString,
                position.x,
                position.y + fontSize / 2 - padding
              ) // データの百分率
            })
          }
        })
      },
    })
    this.renderChart(this.result, this.options)
  },
}
</script>
