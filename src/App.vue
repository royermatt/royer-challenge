<template>
  <div id="app">
    <Header />
    <div class="themes">
        <span
          v-bind:class="{active:theme === 'default'}" @click="setTheme('default')">Default</span>
        <span v-bind:class="{ active:theme === 'nfhs'}" @click="setTheme('nfhs')">NFHS</span>
    </div>
    <h3 class="page-title">Events List</h3>
    <Filters v-model="filters" />
    <EventsList
      :theme="theme"
      :assocation="filters.assocation"
      :dateRange="filters.dateRange" />
  </div>
</template>

<script>
  import Header from './components/Header.vue';
  import Filters from './components/Filters.vue';
  import EventsList from './components/EventsList.vue';

  export default {
    name: 'App',
    components: { Header, Filters, EventsList },
    data() {
      return {
        theme: "default",
        filters: {
          assocation: "18bad24aaa",
          dateRange: {
            startDate: null,
            endDate: null,
          }
        }
      }
    },
    methods: {
      setTheme(value) {
        this.theme = value;
      }
    }
  }
</script>

<style lang="scss">
  @import url("https://fonts.googleapis.com/css?family=Montserrat:400,500,600,800");

  body {
    background: #09121a;
    min-width: 380px;
    margin: 0;
  }

  #app {
    font-family: Montserrat, Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #bebebe;

    .themes {
      text-align: right;
      margin: 20px;
      font-size: 0.8em;

      span {
        padding: 8px;

        &.active {
          font-weight: bold;
          color: #ffffff;
        }

        &:hover {
          cursor: pointer;
        }
      }
    }
    
    .page-title {
        font-size: 40px;
        font-weight: 500;
        color: #fff;
        margin-bottom: 24px;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
    }
  }
</style>
