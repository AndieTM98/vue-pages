<template>
  <div id="app" class="container">
    <h1>Random Quote Generator</h1>

    <!-- Display the quote or a loading message -->
     <p v-if="loading">Loading...</p>
     <p v-else>"{{ quote }}"</p>
     <p class="author">— {{ author }}</p>
     <button @click="fetchQuote">Get New Quote</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      quote: '',
      author: '',
      loading: false
    }
  },
  methods: {
    async fetchQuote() {
      this.loading = true
      try {
        const response = await axios.get('https://quotes15.p.rapidapi.com/quotes/random/?language_code=en', {
        headers: {
          'X-RapidAPI-Host': 'quotes15.p.rapidapi.com',
          'X-RapidAPI-Key': '4fab6fc910msh62a2d20d3bc605bp14cb6ajsn640e6bc6e3e5',
        }
        })
        // Update quote and author with the response data
        this.quote = response.data.content
        this.author = response.data.originator.name
        this.loading = false
      } catch (error) {
        console.error('Error fetching quote:', error)
        this.quote = 'Failed to fetch quote.'
        this.author = ''
      } finally {
        this.loading = false
      }
    }
  },
  mounted() {
    this.fetchQuote()
  }
}
</script>

<style>
.container {
  text-align: center;
  font-family: Arial, Helvetica, sans-serif;
  margin-top: 50px;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

.author {
  color: #555;
  margin-top: 5px;
  font-style: italic;
}
</style>
