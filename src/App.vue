<script setup>
import { ref, computed } from 'vue'

const amount = ref(0)
const people = ref(1)
const tipPercent = ref(10)

const tipAmount = computed(() => {
  return (amount.value || 0) * (tipPercent.value / 100)
})

const total = computed(() => {
  return (amount.value || 0) + tipAmount.value
})

const perPerson = computed(() => {
  const count = people.value >= 1 ? people.value : 1
  return total.value / count
})

function reset() {
  amount.value = 0
  people.value = 1
  tipPercent.value = 10
}

function format(n) {
  return Math.round(n * 100) / 100
}
</script>

<template>
  <div class="card">
    <h1>分帳／小費計算機</h1>

    <label>
      帳單金額
      <input v-model.number="amount" type="number" min="0" placeholder="例如 1200" />
    </label>

    <label>
      人數
      <input v-model.number="people" type="number" min="1" />
    </label>

    <label>
      小費比例：{{ tipPercent }}%
      <input v-model.number="tipPercent" type="range" min="0" max="30" step="1" />
    </label>

    <div class="result">
      <p>小費金額：<strong>{{ format(tipAmount) }}</strong></p>
      <p>總金額：<strong>{{ format(total) }}</strong></p>
      <p class="per-person">每人應付：<strong>{{ format(perPerson) }}</strong></p>
    </div>

    <button @click="reset">重設</button>
  </div>
</template>

<style scoped>
.card {
  max-width: 360px;
  margin: 60px auto;
  padding: 24px;
  border: 1px solid #ccc;
  border-radius: 12px;
  text-align: left;
}

h1 {
  text-align: center;
  font-size: 1.6rem;
}

label {
  display: block;
  margin: 16px 0;
}

input {
  display: block;
  width: 100%;
  box-sizing: border-box;
  margin-top: 6px;
  padding: 8px;
  font-size: 1rem;
}

.result {
  margin: 20px 0;
  padding: 12px 16px;
  background: #f3f6fa;
  border-radius: 8px;
  color: #222;
}

.per-person {
  font-size: 1.3rem;
}

button {
  width: 100%;
  padding: 10px;
  font-size: 1rem;
  cursor: pointer;
}
</style>