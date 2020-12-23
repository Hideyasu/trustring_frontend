<template>
  <v-flex>
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
      questions: [
        {
          type: '分析型',
          content: '筋道を通す',
          value: 0,
        },
        {
          type: '分析型',
          content: '理性的に考える',
          value: 0,
        },
        {
          type: '分析型',
          content: '知的に考えるのを好む',
          value: 0,
        },
        {
          type: '分析型',
          content: '客観的に物事をみる',
          value: 0,
        },
        {
          type: '分析型',
          content: '理論的な考え方をする',
          value: 0,
        },
        {
          type: '分析型',
          content: '懐疑的',
          value: 0,
        },
        {
          type: '分析型',
          content: '批判的',
          value: 0,
        },
        {
          type: '分析型',
          content: '研究熱心',
          value: 0,
        },
        {
          type: '分析型',
          content: '矛盾は追求したい',
          value: 0,
        },
        {
          type: 'コンセプト型',
          content: '創意工夫に富む',
          value: 0,
        },
        {
          type: 'コンセプト型',
          content: '独創的',
          value: 0,
        },
        {
          type: 'コンセプト型',
          content: '革新的',
          value: 0,
        },
        {
          type: 'コンセプト型',
          content: '想像力が豊か',
          value: 0,
        },
        {
          type: 'コンセプト型',
          content: '直感でアイデアを判断する',
          value: 0,
        },
        {
          type: 'コンセプト型',
          content: '幅広い視野を持つ',
          value: 0,
        },
        {
          type: 'コンセプト型',
          content: '人と違うことを好む',
          value: 0,
        },
        {
          type: 'コンセプト型',
          content: '変化を好む',
          value: 0,
        },
        {
          type: 'コンセプト型',
          content: '飽きっぽい',
          value: 0,
        },
        {
          type: '構造型',
          content: '細部にこだわる',
          value: 0,
        },
        {
          type: '構造型',
          content: '規律を守る',
          value: 0,
        },
        {
          type: '構造型',
          content: '秩序を重んじる',
          value: 0,
        },
        {
          type: '構造型',
          content: 'ルールを守る',
          value: 0,
        },
        {
          type: '構造型',
          content: '手順を重視する',
          value: 0,
        },
        {
          type: '構造型',
          content: '現実的',
          value: 0,
        },
        {
          type: '構造型',
          content: '整頓が得意',
          value: 0,
        },
        {
          type: '構造型',
          content: '伝統重視',
          value: 0,
        },
        {
          type: '構造型',
          content: '行動の予測がつきやすい',
          value: 0,
        },
        {
          type: '社交型',
          content: '人間関係を重視する',
          value: 0,
        },
        {
          type: '社交型',
          content: '人の気持ちがすぐわかる',
          value: 0,
        },
        {
          type: '社交型',
          content: '愛想がいい',
          value: 0,
        },
        {
          type: '社交型',
          content: '情が深い',
          value: 0,
        },
        {
          type: '社交型',
          content: '思いやりがある',
          value: 0,
        },
        {
          type: '社交型',
          content: '感情移入しやすい',
          value: 0,
        },
        {
          type: '社交型',
          content: '感情的',
          value: 0,
        },
        {
          type: '社交型',
          content: 'サポート役に回る',
          value: 0,
        },
        {
          type: '社交型',
          content: '直感で人を判断する',
          value: 0,
        },
      ],
    }
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
