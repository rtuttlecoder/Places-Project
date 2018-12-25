<template>
    <div class="here-map">
        <div ref="map" v-bind:style="{ width: width, height: height }"></div>
    </div>
</template>

<script>
export default {
  name: "HereMap",
  data() {
    return {
      map: {},
      platform: {},
      ui: {},
      search: {}
    };
  },
  props: {
    appId: String,
    appCode: String,
    lat: String,
    lng: String,
    width: String,
    height: String
  },
  created() {
    this.platform = new H.service.Platform({
      app_id: this.appId,
      app_code: this.appCode
    });
    this.search = new H.places.Search(this.platform.getPlacesService());
  },
  mounted() {
    var defaultLayers = this.platform.createDefaultLayers();
    this.map = new H.Map(this.$refs.map, defaultLayers.normal.map, {
      zoom: 10,
      center: { lng: this.lng, lat: this.lat }
    });
    var behavior = new H.mapevents.Behavior(
      new H.mapevents.MapEvents(this.map)
    );
    this.ui = H.ui.UI.createDefault(this.map, defaultLayers);
  },
  methods: {
    places(query) {
      this.map.removeObjects(this.map.getObjects());
      this.search.request(
        { q: query, at: this.lat + "," + this.lng },
        {},
        data => {
          for (var i = 0; i < data.results.items.length; i++) {
            this.dropMarker(
              {
                lat: data.results.items[i].position[0],
                lng: data.results.items[i].position[1]
              },
              data.results.items[i]
            );
          }
        },
        error => {
          console.error(error);
        }
      );
    },
    dropMarker(coordinates, data) {
      var marker = new H.map.Marker(coordinates);
      marker.setData("<p>" + data.title + "<br>" + data.vicinity + "</p>");
      marker.addEventListener(
        "tap",
        event => {
          var bubble = new H.ui.InfoBubble(event.target.getPosition(), {
            content: event.target.getData()
          });
          this.ui.addBubble(bubble);
        },
        false
      );
      this.map.addObject(marker);
    }
  }
};
</script>

<style scoped>
</style>
