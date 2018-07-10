<template>
  <div id="app">
    <div style="width: 600px; margin: 0 auto">
      <h1>搜索组件</h1>
      <v-search
        :search="searchData"
        :searchItem="[]"
      ></v-search>
    </div>
    <div style="width: 944px; margin: 0 auto;">
      <h1>两侧联动</h1>
      <v-scroll/>
    </div>
  </div>
</template>

<script>
  import Search from './components/search';
  import Scroll from './components/scroll'

  export default {
    name: 'App',
    components: {
      'v-search': Search,
      'v-scroll': Scroll
    },
    data() {
      return {};
    },
    computed: {
      searchData() {
        let data = [];
        for (let i of [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]) {
          for (let j of [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]) {
            data[i * j] = {
              id: i * j,
              name: 'name' + `${Math.random()}`.substr(3, 18)
            };
          }
        }

        return data.filter(item => item);
      }
    }
  };
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin: 100px auto;
    /*width: 600px;*/
  }
</style>
