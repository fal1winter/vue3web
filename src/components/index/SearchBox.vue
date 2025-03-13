<template>
  <div class="search-box">
    <a-input-search
      v-model:value="searchContentTemp"
      :placeholder="$t('common.searchPlaceholder')"
      style="min-width: 100px; width: 100%"
      @search="onSearch"
    >

	    </a-input-search>
	<a-button-group>
	  <a-button @click="setSearchEngine('baidu')">百度</a-button>
	  <a-button @click="setSearchEngine('google')">谷歌</a-button>
	  <a-button @click="setSearchEngine('googleScholar')">谷歌学术</a-button>
	  <a-button @click="setSearchEngine('null')">papertalk</a-button>
	</a-button-group>
  </div>
</template>

<script>
export default {
  name: 'SearchBox',
  data() {
    return {
      searchContentTemp: this.searchContent,
	  activeSearchEngine: null, // 用于记录用户选择的搜索引擎
    };
  },
  props: {
    searchContent: {
      type: String,
      default: '',
    },
  },
  watch: {
    searchContent(newVal) {
      this.searchContentTemp = newVal;
    },
  },

  methods: {
    onSearch(value) {
      if (this.activeSearchEngine) {
        this.handleExternalSearch(value);
      } else {
        this.$router.push({path: "/search", query: {query: value}});
      }
      this.activeSearchEngine = null; // 重置选择
    },
    setSearchEngine(engine) {
      this.activeSearchEngine = engine;
    },
    handleExternalSearch(value) {
      let url = '';
      switch (this.activeSearchEngine) {
        case 'baidu':
          url = `https://www.baidu.com/s?wd=${encodeURIComponent(value)}`;
          break;
        case 'google':
          url = `https://www.google.com/search?q=${encodeURIComponent(value)}`;
          break;
        case 'googleScholar':
          url = `https://scholar.google.com/scholar?q=${encodeURIComponent(value)}`;
          break;
		
      }
      if (url) {
        window.open(url, '_blank');
      }
    },
  },
};
</script>

<style scoped>
.search-box {
  width: 100%;
}
.ant-btn-group {
  margin-left: 10px;
}

.ant-btn {
  padding: 0 10px;
  height: 32px;
  font-size: 12px;
}
</style>