<template>
  <div class="search-results">
    <h2>AI助手</h2>
    <div v-if="loading" class="loading">
      <div class="spinner"></div>
      Thinking...
    </div>
    <div v-else-if="results" class="results-content">
      <div v-html="formattedResults"></div>
    </div>
    <div v-else>No results to display</div>
  </div>
</template>

<script>
export default {
  name: 'SearchResults',
  props: {
    results: String,
    loading: Boolean,
  },
  computed: {
    formattedResults() {
      return this.results.replace(/\n/g, '<br>');
    },
  },
};
</script>

<style scoped>
.search-results {
  background-color: white;
  border-radius: 8px; /* 增加整体圆角 */
  padding: 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 45vh;
  margin: 0 auto;
}

h2 {
  color: #FF6F00;
  margin-bottom: 15px;
}

.loading {
  display: flex;
  align-items: center;
  justify-content: center;
  color: #666;
  min-height: 100px;
}

.spinner {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #FF6F00;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: spin 1s linear infinite;
  margin-right: 10px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.results-content {
  line-height: 1.6;
  max-height: 500px;
  overflow-y: auto;
}

/* 移动端样式 */
@media screen and (max-width: 768px) {
  .search-results {
    max-width: 75vw;
    width: 75vw;
    border-radius: 8px; /* 增加整体圆角 */
    padding: 15px;
  }

  .results-content {
    max-height: calc(100vh - 100px); /* 调整最大高度，留出一些空间给标题和其他元素 */
  }
}
</style>