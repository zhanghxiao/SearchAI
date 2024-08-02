<template>
  <div class="source-list">
    <h2>搜索结果</h2>
    <div v-if="loading" class="loading">
      <div class="spinner"></div>
      Searching...
    </div>
    <ul v-else-if="sources && sources.length">
      <li v-for="(source, index) in sources" :key="index">
        <a :href="source.href" target="_blank">{{ source.title }}</a>
        <p>{{ truncateBody(source.body) }}</p>
      </li>
    </ul>
    <div v-else>暂无搜索结果</div>
  </div>
</template>

<script>
export default {
  name: 'SourceList',
  props: {
    sources: Array,
    loading: Boolean,
  },
  methods: {
    truncateBody(body, maxLength = 100) {
      if (body.length <= maxLength) return body;
      return body.slice(0, maxLength) + '...';
    }
  }
};
</script>

<style scoped>
.source-list {
  flex: 1;
  background-color: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

h2 {
  color: #FF6F00;
  margin-bottom: 15px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 20px;
  padding-bottom: 20px;
  border-bottom: 1px solid #eee;
}

li:last-child {
  border-bottom: none;
}

a {
  color: #FF6F00;
  text-decoration: none;
  font-weight: bold;
  font-size: 1.1em;
}

a:hover {
  text-decoration: underline;
}

p {
  margin: 5px 0 0;
  color: #666;
  font-size: 0.9em;
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
</style>