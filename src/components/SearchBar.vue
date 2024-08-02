<template>
  <div class="search-bar">
    <input 
      v-model="query" 
      @keyup.enter="search"
      type="text" 
      placeholder="Ask a question..."
    >
    <button @click="search">Search</button>
  </div>
</template>

<script>
export default {
  name: 'SearchBar',
  data() {
    return {
      query: '',
    };
  },
  methods: {
    search() {
      if (this.query.trim()) {
        this.$emit('search', this.query);
      }
    },
  },
};
</script>

<style scoped>
.search-bar {
  display: flex;
  margin-bottom: 30px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 50px;
  overflow: hidden;
}

input {
  flex-grow: 1;
  padding: 15px 25px;
  font-size: 18px;
  border: none;
  outline: none;
}

button {
  padding: 15px 30px;
  font-size: 18px;
  background-color: #FF6F00;
  color: white;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #FF8F00;
}
</style>