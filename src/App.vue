<script setup>
import {ref, computed} from 'vue'

let selectedApi = ref('gpt-4-1106-preview')
let inputToken = ref(0)
let outputToken = ref(0)
let selectedCurrency = ref('JPY')
const currencies = ref([
    'JPY',
    'EUR',
    'USD',
])

const currencyRate = ref({
    USD: 1,
    EUR: 0.85,
    JPY: 150.0,
})

const apis = ref([
  {
    name: 'GPT-4 Turbo',
    model: 'gpt-4-1106-preview',
    input: 0.01,
    input_count: 1000,
    output: 0.01,
    output_count: 1000
  },
  {
    name: 'GPT-4',
    model: 'gpt-4',
    input: 0.03,
    input_count: 1000,
    output: 0.06,
    output_count: 1000
  },
  {
    name: 'GPT-4',
    model: 'gpt-4-32k',
    input: 0.06,
    input_count: 1000,
    output: 0.12,
    output_count: 1000
  }
])

const changeSelectedApi = (event) => {
  selectedApi.value = event.target.value
}

const selectedApiData = computed(() => {
  return apis.value.find(api => api.model === selectedApi.value)
})

const calculate = (input,inputCount,inputToken,) => {
  return (input * inputToken / inputCount).toFixed(2)
}

const localCalculate = (input,inputCount,inputToken,) => {
  return (input * inputToken / inputCount * currencyRate.value[selectedCurrency.value]).toFixed(2)
}

</script>

<template>
  <header>
    <h1>OpenAI API Calculator</h1>
    <lead>Calculator for OpenAI API usage</lead>
  </header>
  <main>
    <select @change="changeSelectedApi">
      <option v-for="api in apis" :key="api.model">{{ api.model }}</option>
    </select>
    <div v-for="api in apis" :key="api.model">
      <div v-if="api.model === selectedApi">
        <h2>{{ api.name }}</h2>
        <p>Input: {{ api.input }} / {{ api.input_count }}</p>
        <p v-if="api.output">Output: {{ api.output }} / {{ api.output_count }}</p>
        <hr>
      </div>
    </div>
    {{ selectedCurrency }}
    {{ currencyRate[selectedCurrency] }}
    <div>
      <select @change="selectedCurrency">
        <option v-for="currency in currencies" :key="currency">{{ currency }}</option>
      </select>
    </div>
    <div>
      <h2>使用量</h2>
      <label id="inputToken">Input</label>
      <input v-model="inputToken" type="number" id="inputToken" name="inputToken" min="0"> Tokens
      <div>
        ${{ calculate(selectedApiData.input, selectedApiData.input_count, inputToken) }}→
        {{ localCalculate(selectedApiData.input, selectedApiData.input_count, inputToken) }} {{ selectedCurrency }}
      </div>
      <label id="outputToken">Output</label>
      <input v-model="outputToken" type="number" id="outputToken" name="outputToken" min="0"> Tokens
      <div>
        ${{ calculate(selectedApiData.output, selectedApiData.output_count, outputToken) }}→
        {{ localCalculate(selectedApiData.output, selectedApiData.output_count, outputToken) }} {{ selectedCurrency }}
      </div>
    </div>
  </main>
  <footer>
  </footer>
</template>