<template>
  <div id="app">
    <header>
      <img src="@/assets/logo.png" alt="AI Search Engine Logo" class="logo">
<!--      <h1>AI Search Engine</h1> -->
      <h1>AI 智能搜索引擎</h1>
    </header>
    <SearchBar @search="performSearch" />
    <div class="content">
      <SearchResults :results="searchResults" :loading="loading" />
      <SourceList :sources="sources" :loading="loading" />
    </div>
  </div>
</template>

<script>
import SearchBar from './components/SearchBar.vue';
import SearchResults from './components/SearchResults.vue';
import SourceList from './components/SourceList.vue';

export default {
  name: 'App',
  components: {
    SearchBar,
    SearchResults,
    SourceList,
  },
  data() {
    return {
      searchResults: '',
      sources: [],
      loading: false,
    };
  },
  methods: {
    async performSearch(query) {
      this.loading = true;
      this.searchResults = '';
      this.sources = [];

      const searchApiUrl = process.env.VUE_APP_SEARCH_API_URL;
      const aiApiUrl = process.env.VUE_APP_AI_API_URL;
      const apiKey = process.env.VUE_APP_API_KEY;

      try {
        // First, perform the search
        const searchResponse = await fetch(searchApiUrl, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            question: query,
            api_key: apiKey,
          }),
        });

        const searchData = await searchResponse.json();
        if (searchData.search_results) {
          this.sources = searchData.search_results;
        }

        // Then, use the AI to process the results
        const aiResponse = await fetch(aiApiUrl, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${apiKey}`,
          },
          body: JSON.stringify({
            model: "gpt-4o-mini",
            messages: [
              { role: "system", content: "You are a helpful assistant." },
              { role: "user", content: query }
            ],
            stream: true,
          }),
        });

        const reader = aiResponse.body.getReader();
        const decoder = new TextDecoder();
// eslint-disable-next-line no-constant-condition
        while (true) {
          const { done, value } = await reader.read();
          if (done) break;

          const chunk = decoder.decode(value);
          const lines = chunk.split('\n').filter(line => line.trim() !== '');

          for (const line of lines) {
            if (line.startsWith('data: ')) {
              const data = JSON.parse(line.slice(6));
              if (data.choices && data.choices[0].delta && data.choices[0].delta.content) {
                this.searchResults += data.choices[0].delta.content;
              }
            }
          }
        }
      } catch (error) {
        console.error('Error performing search:', error);
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: 'Roboto', sans-serif;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  background-color: #FFF8E1; /* 暖色背景 */
  min-height: 100vh;
}

header {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 30px;
}

.logo {
  width: 50px;
  height: 50px;
  margin-right: 15px;
}

h1 {
  font-size: 2.5em;
  color: #FF6F00; /* 暖色调标题 */
}

.content {
  display: flex;
  gap: 30px;
}

@media (max-width: 768px) {
  .content {
    flex-direction: column;
  }
}
</style>