<template>
  <v-container fill-height fluid class="deep-purple lighten-2 pa-0">
    <v-layout>
      <v-flex>
        <v-card color="purple" height="100%">
          <l-map
            ref="map"
            :options="mapOptions"
            :zoom.sync="$vuetify.breakpoint.smAndDown ? smallZoom : zoom"
            :center.sync="center"
            class="mapContainer"
          >
            <l-control-zoom
              v-if="$vuetify.breakpoint.mdAndUp"
              position="topleft"
            />
            <l-tile-layer :url="arcLight" />
            <!-- :url="lightTiles" -->
            <!-- :url="standardTiles" -->
            <!-- :url="darkTiles" -->
            <l-geo-json
              ref="geojson"
              :geojson="districts"
              :options="options"
              :options-style="styleFunction"
            />
            <v-container fill-height fluid>
              <v-layout justify-end>
                <v-flex class="overlayContainer" xs4>
                  <v-layout column>
                    <v-flex>
                      <v-card dark color="primary" class="elevation-10 mb-5">
                        <v-toolbar
                          card
                          dark
                          color="primary"
                          class="justify-space-between"
                        >
                          <v-btn :disabled="yearSelected === 0" icon>
                            <v-icon>mdi-chevron-left</v-icon>
                          </v-btn>
                          <v-tabs
                            v-model="yearSelected"
                            hide-slider
                            color="primary"
                            grow
                            class="mx-2"
                            active-class="primary lighten-1"
                          >
                            <v-tab v-for="year in years" :key="year">
                              <h1 class="subheading white--text">
                                {{ year }}
                              </h1>
                            </v-tab>
                          </v-tabs>
                          <v-btn icon :disabled="yearSelected === 2">
                            <v-icon>mdi-chevron-right</v-icon>
                          </v-btn>
                        </v-toolbar>
                        <v-card-text>
                          <v-select
                            hide-details
                            solo-inverted
                            flat
                            item-text="name"
                            item-value="value"
                            label="Select dataset"
                          />
                        </v-card-text>
                        <v-card-text class="pt-0">
                          <v-btn
                            block
                            :disabled="!districtSelected"
                            color="white"
                            :light="districtSelected"
                            depressed
                            @click="resetMap"
                          >
                            Reset map
                          </v-btn>
                        </v-card-text>
                        <v-card-actions class="justify-space-between">
                          <span
                            >{{
                              $vuetify.breakpoint.smAndDown ? smallZoom : zoom
                            }}
                          </span>
                          <span>{{ center }}</span>
                        </v-card-actions>
                      </v-card>
                    </v-flex>
                    <v-flex v-if="selectedDistrict.length">
                      <v-card
                        dark
                        height="300"
                        color="primary"
                        class="elevation-10"
                      >
                        <v-card-title
                          ><h2 class="subheading">
                            {{ selectedDistrict.NAME }}
                          </h2></v-card-title
                        >
                      </v-card>
                    </v-flex>
                  </v-layout>
                </v-flex>
              </v-layout>
            </v-container>
          </l-map>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import districts1516 from '@/assets/data/mde-districts-1516.json'

export default {
  layout: 'map',
  data() {
    return {
      center: [32.8, -89.862671],
      originalCenter: [32.8, -89.862671],
      districts1516,
      selectedDistrict: [],
      zoom: 7.5,
      mapOptions: { zoomSnap: 0.5, zoomControl: false, zoomDelta: 0.5 },
      smallZoom: 9,
      standardTiles: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      lightTiles:
        'https://cartodb-basemaps-{s}.global.ssl.fastly.net/light_all/{z}/{x}/{y}.png',
      darkTiles:
        'https://cartodb-basemaps-{s}.global.ssl.fastly.net/dark_all/{z}/{x}/{y}.png',
      arcLight:
        'http://services.arcgisonline.com/arcgis/rest/services/Canvas/World_Light_Gray_Base/MapServer/tile/{z}/{y}/{x}'
    }
  },
  computed: {
    options() {
      return {
        onEachFeature: this.onEachFeatureFunction
      }
    },
    zoomStyle() {
      return { fillOpacity: 0.2 }
    },
    districts() {
      return districts1516
    },
    styleFunction() {
      return feature => {
        let fillColor
        const name = feature.properties.NAME

        if (name.length < 26) fillColor = '#f7feae'
        else if (name.length < 29) fillColor = '#BAD3A0'
        else if (name.length < 31) fillColor = '#7EA892'
        else if (name.length < 33) fillColor = '#417D83'
        else fillColor = '#045275'

        return {
          color: fillColor,
          weight: 1,
          fillColor: fillColor,
          fillOpacity: 0.7
        }
      }
    },
    onEachFeatureFunction() {
      return (feature, layer, target) => {
        layer.on('click', () => {
          // eslint-disable-next-line vue/no-side-effects-in-computed-properties
          this.selectedDistrict = feature.properties
          this.map.fitBounds(layer.getBounds().pad(0.04))
          layer.setStyle(this.zoomStyle)
        })
      }
    }
    // onEachFeatureFunction() {
    //   return (feature, layer, target) => {
    //     layer.on({
    //       click: this.handleClick(target)
    //       // mouseover: this.handleMouseOver(),
    //       // mouseout: this.handleMouseOut()
    //     })
    //   }
    // }
  },
  mounted() {
    this.$nextTick(() => {
      this.map = this.$refs.map.mapObject
      // this.map.dragging.disable()
      this.map.setMinZoom(7)
    })
  },
  methods: {
    resetMap() {
      this.map.flyTo(this.originalCenter, 7.5, { duration: 0.2 })
      this.$refs.geojson.mapObject.setStyle(this.styleFunction)
    },
    resetClick(e) {
      this.$refs.geojson.resetStyle()
    },
    handleClick(e) {
      // eslint-disable-next-line no-console
      console.log('handclick')
      this.map.fitBounds(e.getBounds().pad(0.035))
    }
  }
}
</script>

<style>
.overlayContainer {
  z-index: 999;
}
.mapContainer {
  z-index: 0;
}
</style>
