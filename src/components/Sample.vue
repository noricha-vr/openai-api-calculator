<script setup>
</script>

<template>
 カウンター： {{ count }}
  <button @click="increment">+</button>
  <button @click="decrement">-</button>
</template>



<script>
import { ref } from 'vue'

export default {
  setup() {
    const count = ref(0)
    return {
      count
    }
  },
  methods: {
    increment() {
      this.count++
    },
    decrement() {
      this.count--
    }
  }
}
</script>
