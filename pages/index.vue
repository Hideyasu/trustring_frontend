<template>
  <v-flex v-if="questions.length !== 0">
    <v-stepper v-show="!chartShowFlag" v-model="e1" vertical>
      <template v-for="(question, index) in questions">
        <v-stepper-step
          :key="`${index}-step`"
          :complete="e1 > index"
          :step="index"
        >
          {{ question.content }}
          <small>{{ question.type }}</small>
        </v-stepper-step>

        <v-stepper-content :key="`${index}-content`" :step="index">
          <v-card class="mb-12">
            <v-radio-group v-model="question.value" row>
              <v-radio label="あてはまらない" :value="0"></v-radio>
              <v-radio label="どちらともいえない" :value="1"></v-radio>
              <v-radio label="あてはまる" :value="2"></v-radio>
            </v-radio-group>
          </v-card>
          <v-btn
            v-if="index !== questions.length - 1"
            color="primary"
            @click="e1 = index + 1"
          >
            次へ
          </v-btn>
          <v-btn
            v-else-if="index === questions.length - 1"
            color="primary"
            @click="showResult"
          >
            結果をみる
          </v-btn>
          <v-btn v-if="index !== 0" color="secondary" @click="e1 = index - 1">
            戻る
          </v-btn>
        </v-stepper-content>
      </template>
    </v-stepper>
    <v-card v-show="chartShowFlag">
      <v-card-title class="headline primary white--text" primary-title>
        診断結果
      </v-card-title>
      <v-card-text class="mt-4">
        <PieChart :result="result" />
      </v-card-text>
      <v-divider></v-divider>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="primary" text @click="chartShowFlag = false">
          診断にもどる
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-flex>
</template>

<script>
import PieChart from '@/components/PieChart.vue'

export default {
  components: {
    PieChart,
  },
  data() {
    return {
      e1: 0,
      chartShowFlag: false,
      result: {},
      questions: [],
    }
  },
  async mounted() {
    await this.$axios
      .get('https://trustring-backend.herokuapp.com/v1/question')
      .then((response) => {
        this.questions = response.data.questions
      })
  },
  methods: {
    showResult() {
      const analysisTotal = this.questions.reduce(function (sum, element) {
        if (element.type !== '分析型') {
          return sum
        }
        return sum + element.value
      }, 0)
      const conceptTotal = this.questions.reduce(function (sum, element) {
        if (element.type !== 'コンセプト型') {
          return sum
        }
        return sum + element.value
      }, 0)
      const structureTotal = this.questions.reduce(function (sum, element) {
        if (element.type !== '構造型') {
          return sum
        }
        return sum + element.value
      }, 0)
      const communicationTotal = this.questions.reduce(function (sum, element) {
        if (element.type !== '社交型') {
          return sum
        }
        return sum + element.value
      }, 0)
      this.result = {
        labels: ['分析型', 'コンセプト型', '構造型', '社交型'],
        datasets: [
          {
            label: ['Data One'],
            backgroundColor: ['#0095d9', '#ffd900', '#3eb370', '#e60033'],
            data: [
              analysisTotal,
              conceptTotal,
              structureTotal,
              communicationTotal,
            ],
          },
        ],
      }
      this.chartShowFlag = true
    },
  },
}
</script>
