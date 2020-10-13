<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card>
        <v-card-title class="headline">
          ยินดีเข้าสู่การสอบออนไลน์
        </v-card-title>
        <v-alert dense text type="success" v-if="point">
          คุณได้คะแนน <strong>{{ point }}</strong>
        </v-alert>

        <v-card-text>
          <v-list v-bind:disabled="check">
            <v-list-item v-for="(item, i) in q" :key="i">
              <v-list-item-content>
                <v-list-item-title>{{ item.q_name }}</v-list-item-title>
                <v-list-item-subtitle>
                  <v-text-field
                    dense
                    v-bind:append-outer-icon="
                      item.statue === ''
                        ? 'mdi-help-circle'
                        : item.statue
                        ? 'mdi-checkbox-marked'
                        : 'mdi-close-circle'
                    
                    "
                    v-model="item.q_answer"
                    
                    v-bind:value="check ? item.q_real_answer: ''"
                  ></v-text-field>
                </v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn color="error" large @click="handleAnswerClear">
            <v-icon>mdi-backup-restore</v-icon>
            รีเซ็ทคำตอบ
          </v-btn>
          <v-btn color="primary" large @click="handleAnswerCheck">
            <v-icon>mdi-arrow-right</v-icon>
            ตรวจคำตอบ
          </v-btn>
        </v-card-actions>
        <v-overlay :value="overlay">
          <v-progress-circular indeterminate size="64"></v-progress-circular>
        </v-overlay>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      item: 1,
      point: false,
      check: false,
      overlay: false,
      q: [
        { q_name: '1+1= ?', q_answer: '', q_real_answer: '2', statue: '' },
        { q_name: '2+2= ?', q_answer: '', q_real_answer: '4', statue: '' },
        { q_name: '3+3= ?', q_answer: '', q_real_answer: '6', statue: '' },
        { q_name: '4+4= ?', q_answer: '', q_real_answer: '8', statue: '' },
        { q_name: '5+5= ?', q_answer: '', q_real_answer: '10', statue: '' },
      ],
    }
  },
  watch: {
    overlay(val) {
      val &&
        setTimeout(() => {
          this.overlay = false
        }, 1000)
    },
  },
  methods: {
    handleAnswerClear() {
      this.overlay = true
      this.q.forEach((value, index) => {
        value.statue = ''
        value.q_answer = ''
      })
      this.check = false
    },
    handleAnswerCheck() {
      this.overlay = true
      this.q.forEach((value, index) => {
        value.statue = ''
        if (value.q_answer === value.q_real_answer) {
          value.statue = true
          this.point += 1
        } else {
          value.statue = false
        }
        
        value.q_answer = `ตอบ ${value.q_answer} คำตอบคือ ${value.q_real_answer}`
      })
      this.check = true
    },
  },
}
</script>

