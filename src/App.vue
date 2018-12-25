<template>
  <div id="app">
    <input type="text" v-model="query" />
    <button type="button" @click="search()">Search</button>
    <HereMap ref="map" appId="SVQ0TdpNoUI1IVmBXgik" appCode="Dq7p9t67FwY9yX8pnojDLw" lat="43.616" lng="-116.206" width="100%" height="800px" />
  </div>
</template>

<script>
import HereMap from "./components/HereMap.vue";

export default {
  name: "app",
  components: {
    HereMap
  },
  data() {
    return {
      query: "Starbucks"
    };
  },
  methods: {
    search() {
      this.$refs.map.places(this.query);
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
}
</style>
