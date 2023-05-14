<template>
  <table id="tables">
    <tr v-for="comment in comments" :key="comment.id">
      <th>{{ comment.email }}</th>
      <th>{{ comment.id }}</th>
    </tr>
  </table>
  <infinite-loading @infinite="loadData" />
</template>
<script>
import InfiniteLoading from "v3-infinite-loading";
import "v3-infinite-loading/lib/style.css";
export default {
  name: "HelloWorld",
  components: {
    "infinite-loading": InfiniteLoading,
  },
  data() {
    return {
      comments: [],
      page: 1,
    };
  },
  methods: {
    async loadData(state) {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/comments?_limit=4&_page=" +
          this.page
        );
        const json = await response.json();
        if (json.length < 4) state.complete();
        else {
          this.comments.push(...json);
          state.loaded();
        }
        this.page++;
      } catch (error) {
        state.error();
      }
    },
  },
};
</script>
<style scoped>
  #tables {
    font-family: Arial, Helvetica, sans-serif;
    border-collapse: collapse;
    width: 100%;
  }
  #tables td,
  #tables th {
    border: 1px solid #ddd;
    padding: 8px;
  }
  #tables tr:nth-child(even) {
    background-color: #f2f2f2;
  }
  #tables tr:hover {
    background-color: #ddd;
  }
  #tables th {
    padding-top: 12px;
    padding-bottom: 12px;
    text-align: left;
    background-color: #04AA6D;
    color: white;
  }
</style>

