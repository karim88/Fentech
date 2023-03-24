<template>
  <div v-if="getResults" id="app">
    <Graph
      chartType="line"
      :seriesNames="['1. open', '4. close']"
      :results="getResults"
      @graph-click="handle"
    />
    <List :data="descriptions" />
    <Modal
      title="Add Description"
      :show="getIsOpen"
      @close="closeModal()"
      @send="saveData"
    />
  </div>
</template>

<script>
import axios from "axios";
import Graph from "./components/organisms/Graph.vue";
import Modal from "./components/molecules/Modal.vue";
import List from './components/molecules/List.vue';

const API_KEY = `EJ4FAK9BA9E3ORQ6`;
const API_URL = `https://www.alphavantage.co/query?function=TIME_SERIES_DAILY_ADJUSTED&symbol=MSFT&apikey=${API_KEY}`;

export default {
  name: "App",
  components: {
    Graph,
    Modal,
    List,
  },
  data() {
    return {
      results: [],
      isOpen: false,
      descriptions: [],
      currentPoint: {},
    };
  },
  computed: {
    getResults() {
      return this.results?.data;
    },
    getIsOpen() {
      return this.isOpen;
    },
  },
  mounted() {
    if (localStorage.getItem("descriptions")) {
      this.descriptions = JSON.parse(localStorage.getItem("descriptions"));
    }
    this.getData().then((data) => {
      this.results = this.json(data);
    });
  },
  methods: {
    getData: () => {
      return axios.get(API_URL);
    },
    json: (data) => {
      return JSON.parse(JSON.stringify(data));
    },
    handle(event) {
      this.currentPoint = event;
      this.isOpen = true;
    },
    closeModal() {
      this.isOpen = false;
    },
    saveData(event) {
      if (event) {
        this.currentPoint.description = event;
        this.descriptions.push(this.currentPoint);
        localStorage.setItem("descriptions", JSON.stringify(this.descriptions));
      }
      this.isOpen = false;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  padding: 20px;
  background-color: #ffe;
}
</style>
