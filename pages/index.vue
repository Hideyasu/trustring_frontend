<template>
  <v-flex>
    <v-card v-if="!isNameSet">
      <v-card-title class="headline primary white--text" primary-title>
        簡易EG診断
      </v-card-title>
      <v-card align="center">
        <v-card-text class="white--text">
          お名前を入力してください
        </v-card-text>
        <v-form ref="form" @submit.prevent v-model="valid" lazy-validation>
          <v-text-field
            v-model="name"
            label="お名前"
            :rules="nameRules"
            style="margin-left: 15px; margin-right: 15px"
          ></v-text-field>
          <v-card-actions style="display: inline-block">
            <v-btn color="primary" @click="setIsNameSet">決定</v-btn>
          </v-card-actions>
        </v-form>
      </v-card>
    </v-card>
    <v-container v-else-if="!chartShowFlag">
      <v-row>
        <v-col
          v-for="question in questions.slice(
            (page - 1) * separateNumber,
            page * separateNumber
          )"
          :key="question.content"
          cols="12"
          sm="4"
        >
          <v-card class="mx-auto">
            <v-card-title>{{ question.content }}</v-card-title>

            <v-divider class="mx-4"></v-divider>

            <v-card-actions>
              <v-radio-group v-model="question.value" style="margin-left: 5px">
                <v-radio
                  v-for="radioComponent in radioComponents"
                  :key="radioComponent.label"
                  :label="radioComponent.label"
                  :value="radioComponent.value"
                ></v-radio>
              </v-radio-group>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
      <v-row>
        <v-col align="center">
          <v-pagination
            v-model="page"
            :length="questions.length / separateNumber"
          ></v-pagination>
        </v-col>
      </v-row>
      <v-row>
        <v-col align="center">
          <v-btn color="primary" @click="showResult"> 結果をみる </v-btn>
        </v-col>
      </v-row>
      <v-row>
        <v-col align="center">
          <v-btn color="warning" @click="isNameSet = false">
            お名前の入力に戻る
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-card v-else-if="chartShowFlag">
      <v-card-title class="headline primary white--text" primary-title>
        診断結果
      </v-card-title>
      <v-card-text class="mt-4">
        <PieChart :result="result" />
      </v-card-text>
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
      name: '',
      isNameSet: false,
      nameRules: [(v) => !!v || '名前を入力してください'],
      valid: true,
      result: {},
      page: 1,
      separateNumber: 9,
      analysisTotal: 0,
      conceptTotal: 0,
      structureTotal: 0,
      communicationTotal: 0,
      radioComponents: [
        {
          label: 'あてはまらない',
          value: 1,
        },
        {
          label: 'どちらかというとあてはまらない',
          value: 2,
        },
        {
          label: 'どちらともいえない',
          value: 3,
        },
        {
          label: 'どちらかというとあてはまる',
          value: 4,
        },
        {
          label: 'あてはまる',
          value: 5,
        },
      ],
      questions: this.arrayShuffle([
        {
          category: 'EG',
          type: '分析型',
          content: '筋道を通す',
          value: 0,
        },
        {
          category: 'EG',
          type: '分析型',
          content: '理性的に考える',
          value: 0,
        },
        {
          category: 'EG',
          type: '分析型',
          content: '知的に考えるのを好む',
          value: 0,
        },
        {
          category: 'EG',
          type: '分析型',
          content: '客観的に物事をみる',
          value: 0,
        },
        {
          category: 'EG',
          type: '分析型',
          content: '理論的な考え方をする',
          value: 0,
        },
        {
          category: 'EG',
          type: '分析型',
          content: '懐疑的',
          value: 0,
        },
        {
          category: 'EG',
          type: '分析型',
          content: '批判的',
          value: 0,
        },
        {
          category: 'EG',
          type: '分析型',
          content: '研究熱心',
          value: 0,
        },
        {
          category: 'EG',
          type: '分析型',
          content: '矛盾は追求したい',
          value: 0,
        },
        {
          category: 'EG',
          type: 'コンセプト型',
          content: '創意工夫に富む',
          value: 0,
        },
        {
          category: 'EG',
          type: 'コンセプト型',
          content: '独創的',
          value: 0,
        },
        {
          category: 'EG',
          type: 'コンセプト型',
          content: '革新的',
          value: 0,
        },
        {
          category: 'EG',
          type: 'コンセプト型',
          content: '想像力が豊か',
          value: 0,
        },
        {
          category: 'EG',
          type: 'コンセプト型',
          content: '直感でアイデアを判断する',
          value: 0,
        },
        {
          category: 'EG',
          type: 'コンセプト型',
          content: '幅広い視野を持つ',
          value: 0,
        },
        {
          category: 'EG',
          type: 'コンセプト型',
          content: '人と違うことを好む',
          value: 0,
        },
        {
          category: 'EG',
          type: 'コンセプト型',
          content: '変化を好む',
          value: 0,
        },
        {
          category: 'EG',
          type: 'コンセプト型',
          content: '飽きっぽい',
          value: 0,
        },
        {
          category: 'EG',
          type: '構造型',
          content: '細部にこだわる',
          value: 0,
        },
        {
          category: 'EG',
          type: '構造型',
          content: '規律を守る',
          value: 0,
        },
        {
          category: 'EG',
          type: '構造型',
          content: '秩序を重んじる',
          value: 0,
        },
        {
          category: 'EG',
          type: '構造型',
          content: 'ルールを守る',
          value: 0,
        },
        {
          category: 'EG',
          type: '構造型',
          content: '手順を重視する',
          value: 0,
        },
        {
          category: 'EG',
          type: '構造型',
          content: '現実的',
          value: 0,
        },
        {
          category: 'EG',
          type: '構造型',
          content: '整頓が得意',
          value: 0,
        },
        {
          category: 'EG',
          type: '構造型',
          content: '伝統重視',
          value: 0,
        },
        {
          category: 'EG',
          type: '構造型',
          content: '行動の予測がつきやすい',
          value: 0,
        },
        {
          category: 'EG',
          type: '社交型',
          content: '人間関係を重視する',
          value: 0,
        },
        {
          category: 'EG',
          type: '社交型',
          content: '人の気持ちがすぐわかる',
          value: 0,
        },
        {
          category: 'EG',
          type: '社交型',
          content: '愛想がいい',
          value: 0,
        },
        {
          category: 'EG',
          type: '社交型',
          content: '情が深い',
          value: 0,
        },
        {
          category: 'EG',
          type: '社交型',
          content: '思いやりがある',
          value: 0,
        },
        {
          category: 'EG',
          type: '社交型',
          content: '感情移入しやすい',
          value: 0,
        },
        {
          category: 'EG',
          type: '社交型',
          content: '感情的',
          value: 0,
        },
        {
          category: 'EG',
          type: '社交型',
          content: 'サポート役に回る',
          value: 0,
        },
        {
          category: 'EG',
          type: '社交型',
          content: '直感で人を判断する',
          value: 0,
        },
      ]),
    }
  },
  methods: {
    arrayShuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const r = Math.floor(Math.random() * (i + 1))
        const tmp = array[i]
        array[i] = array[r]
        array[r] = tmp
      }
      return array
    },
    setIsNameSet() {
      if (!this.$refs.form.validate()) {
        return
      }
      this.isNameSet = true
    },
    showResult() {
      this.analysisTotal = this.questions.reduce(function (sum, element) {
        if (element.type !== '分析型') {
          return sum
        }
        return sum + element.value
      }, 0)
      this.conceptTotal = this.questions.reduce(function (sum, element) {
        if (element.type !== 'コンセプト型') {
          return sum
        }
        return sum + element.value
      }, 0)
      this.structureTotal = this.questions.reduce(function (sum, element) {
        if (element.type !== '構造型') {
          return sum
        }
        return sum + element.value
      }, 0)
      this.communicationTotal = this.questions.reduce(function (sum, element) {
        if (element.type !== '社交型') {
          return sum
        }
        return sum + element.value
      }, 0)
      this.result = {
        labels: ['コンセプト型', '社交型', '構造型', '分析型'],
        datasets: [
          {
            label: ['Data One'],
            backgroundColor: ['#ffd900', '#e60033', '#3eb370', '#0095d9'],
            data: [
              this.conceptTotal,
              this.communicationTotal,
              this.structureTotal,
              this.analysisTotal,
            ],
          },
        ],
      }

      this.chartShowFlag = true

      const EGTotal =
        this.conceptTotal +
        this.communicationTotal +
        this.structureTotal +
        this.analysisTotal

      this.$axios
        .get(
          'https://script.google.com/macros/s/AKfycbwHkHmAscQsHpU_qm0M_ZYQdid-ZP9haO5SMSRghr5SzdPPRf8/exec',
          {
            params: {
              mode: 'create',
              name: this.name,
              conceptTotal:
                Math.round((this.conceptTotal / EGTotal) * 100) / 100,
              communicationTotal:
                Math.round((this.communicationTotal / EGTotal) * 100) / 100,
              structureTotal:
                Math.round((this.structureTotal / EGTotal) * 100) / 100,
              analysisTotal:
                Math.round((this.analysisTotal / EGTotal) * 100) / 100,
            },
          }
        )
        .then(
          (response) => {
            console.log('送信完了しました')
          },
          (error) => {
            console.log(error)
            alert('送信に失敗しました。時間をおいてから再度やり直してください')
          }
        )
    },
  },
}
</script>
