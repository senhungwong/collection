<template>
  <div style="text-align: center">
    <a-avatar shape="square" src="http://localhost:8080/favicon.ico"/>
    <a-select
      showSearch
      :value="searchInput"
      @search="onSearch"
      @select="onSelect"
      :filterOption="false"
      style="width: 70%; margin: 0 10px"
      :notFoundContent="null"
    >
      <a-icon slot="suffixIcon" type="smile"/>
      <a-select-option v-for="entry in entries" :key="entry.source">{{entry.name}}</a-select-option>
    </a-select>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { SEARCH_QUERY } from "@/queries/search";

export default Vue.extend({
  apollo: {
    entries: {
      query: SEARCH_QUERY,
      variables() {
        return {
          keyword: this.searchInput
        };
      },
      debounce: 300,
      skip() {
        return this.skip;
      },
      update(data) {
        if (this.searchInput === "") {
          return [];
        }

        return data.entries;
      }
    }
  },
  data() {
    return {
      skip: true,
      searchInput: ""
    };
  },
  methods: {
    onSearch(value: string) {
      this.skip = false;
      this.searchInput = value;
    },
    onSelect(url: string) {
      window.open(url, "_blank");
    }
  }
});
</script>
