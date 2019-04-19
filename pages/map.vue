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
            <l-tile-layer :url="lightTiles" />
            <!-- :url="lightTiles" -->
            <!-- :url="standardTiles" -->
            <!-- :url="darkTiles" -->
            <l-geo-json
              ref="geojson"
              :geojson="districts"
              :options="options"
              :options-style="styleFunction"
            />
            <v-container v-if="$vuetify.breakpoint.smAndUp" fluid>
              <v-layout justify-end>
                <v-flex xs4 class="overlayContainer">
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
                        :items="dataSets"
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
                        >{{ $vuetify.breakpoint.smAndDown ? smallZoom : zoom }}
                      </span>
                      <span>{{ center }}</span>
                    </v-card-actions>
                  </v-card>

                  <v-card
                    v-if="districtSelected"
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
                    <v-divider />
                    <v-list dense class="transparent">
                      <v-list-tile
                        v-for="item in markers"
                        :key="item.attributes.OBJECTID"
                      >
                        <v-layout row justify-space-between align-center>
                          <h1 class="body-1" v-text="item.attributes.NAME" />
                          <v-btn flat icon>
                            <v-icon>mdi-chevron-double-right</v-icon>
                          </v-btn>
                        </v-layout>
                      </v-list-tile>
                    </v-list>
                  </v-card>
                </v-flex>
              </v-layout>
            </v-container>
            <l-marker-cluster v-if="markers" ref="schools" chunked-loading>
              <l-marker
                v-for="(school, s) in markers"
                :key="s"
                :lat-lng="[school.geometry.y, school.geometry.x]"
              >
                <l-icon>
                  <v-btn
                    small
                    dark
                    fab
                    :color="
                      school.attributes.NAME.length < 25
                        ? 'deep-orange'
                        : 'deep-purple'
                    "
                  >
                    <v-icon v-if="school.attributes.NAME.length < 25" large>
                      mdi-alpha-a
                    </v-icon>
                    <v-icon v-else large>
                      mdi-alpha-c
                    </v-icon>
                  </v-btn>
                </l-icon>
              </l-marker>
            </l-marker-cluster>
            <v-btn fab dark color="pink" class="overlayContainer"
              ><v-icon>mdi-camera</v-icon></v-btn
            >
          </l-map>
        </v-card>
        <v-footer
          v-if="$vuetify.breakpoint.mdAndDown"
          app
          height="auto"
          color="primary"
        >
          <v-layout column>
            <v-flex>
              <v-slide-y-transition>
                <v-toolbar
                  v-if="menu === 2"
                  dense
                  flat
                  dark
                  color="primary lighten-1"
                  class="justify-space-between"
                >
                  <v-btn
                    :disabled="yearSelected === 0"
                    icon
                    @click="yearSelected--"
                  >
                    <v-icon>mdi-chevron-left</v-icon>
                  </v-btn>
                  <v-tabs
                    v-model="yearSelected"
                    hide-slider
                    color="transparent"
                    dark
                    grow
                    class="mx-2"
                    active-class="primary"
                  >
                    <v-tab v-for="year in years" :key="year">
                      <h1 class="subheading">
                        {{ year }}
                      </h1>
                    </v-tab>
                  </v-tabs>
                  <v-btn
                    icon
                    :disabled="yearSelected === 2"
                    @click="yearSelected++"
                  >
                    <v-icon>mdi-chevron-right</v-icon>
                  </v-btn>
                </v-toolbar>
              </v-slide-y-transition>
            </v-flex>
            <v-flex>
              <v-toolbar dark flat color="transparent">
                <v-btn icon @click="resetMap">
                  <v-icon>mdi-refresh</v-icon>
                </v-btn>
                <v-tabs
                  v-model="menu"
                  color="transparent"
                  active-class="primary lighten-1"
                  grow
                  hide-slider
                >
                  <v-tab>
                    Map
                  </v-tab>
                  <v-tab :disabled="!districtSelected">
                    Schools
                  </v-tab>
                  <v-tab>
                    Years
                  </v-tab>
                  <v-tab>
                    Data
                  </v-tab>
                </v-tabs>
              </v-toolbar>
            </v-flex>
          </v-layout>
        </v-footer>
        <v-dialog :value="menu === 3" fullscreen>
          <v-card light>
            <v-card-title class="py-1 px-0">
              <v-btn icon flat @click="menu = 0">
                <v-icon>mdi-arrow-left</v-icon>
              </v-btn>
              <span class="title font-weight-regular">Select data to view</span>
            </v-card-title>
            <v-divider />
            <v-list>
              <v-list-tile
                v-for="item in dataSets"
                :key="item.name"
                class="my-2"
                @click="handleDataSelect(item.value)"
              >
                <v-list-tile-content>
                  <v-list-tile-title v-text="item.name" />
                  <v-list-tile-sub-title v-text="item.description" />
                </v-list-tile-content>
              </v-list-tile>
            </v-list>
          </v-card>
        </v-dialog>
        <v-dialog :value="menu === 1" fullscreen>
          <v-card light>
            <v-card-title class="py-1 px-0">
              <v-btn icon flat @click="menu = 0">
                <v-icon>mdi-arrow-left</v-icon>
              </v-btn>
              <span class="title font-weight-regular">Schools</span>
            </v-card-title>
            <v-divider />
            <v-list dense>
              <v-list-tile
                v-for="item in markers"
                :key="item.attributes.OBJECTID"
              >
                <v-layout row justify-space-between align-center>
                  <h1 class="body-1" v-text="item.attributes.NAME" />
                  <v-btn flat icon>
                    <v-icon>mdi-chevron-double-right</v-icon>
                  </v-btn>
                </v-layout>
              </v-list-tile>
            </v-list>
          </v-card>
        </v-dialog>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import districts1516 from '@/assets/data/mde-districts-1516.json'
import { features as schools1516 } from '@/assets/data/mde-schools-1516.json'
import 'leaflet.markercluster/dist/MarkerCluster.Default.css'
import Vue2LeafletMarkerCluster from 'vue2-leaflet-markercluster'

export default {
  components: { 'l-marker-cluster': Vue2LeafletMarkerCluster },
  layout: 'map',
  data() {
    return {
      center: [32.8, -89.862671],
      originalCenter: [32.8, -89.862671],
      markers: [],
      districts1516,
      schools1516,
      selectedDistrict: [],
      menu: 0,
      districtSelected: false,
      zoom: 7.5,
      yearSelected: 0,
      dataSets: [
        {
          name: 'Accountability grade',
          description: 'this is a description',
          value: 'grade'
        },
        {
          name: 'Math proficiency',
          description: 'this is a description',
          value: 'MP'
        }
      ],
      years: ['2015/16', '2016/17', '2017/18'],
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
    selectedYear() {
      switch (this.yearSelected) {
        case 0:
          return '2015/2016'
        case 1:
          return '2016/2017'
        case 2:
          return '2017/2018'
        default:
          return '2015/2016'
      }
    },
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
          // eslint-disable-next-line vue/no-side-effects-in-computed-properties
          this.districtSelected = true
          this.getMarkers(layer)
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
      this.map.setMinZoom(5)
    })
  },
  methods: {
    resetMap() {
      if (this.$vuetify.breakpoint.xsAndDown) {
        this.map.flyTo(this.originalCenter, 5, { duration: 0.2 })
        this.$refs.geojson.mapObject.setStyle(this.styleFunction)
      }
      this.map.flyTo(this.originalCenter, 7.5, { duration: 0.2 })
      this.$refs.geojson.mapObject.setStyle(this.styleFunction)
    },
    resetClick(e) {
      this.$refs.geojson.resetStyle()
      this.markers = []
    },
    handleClick(e) {
      // eslint-disable-next-line no-console
      console.log('handclick')
      this.map.fitBounds(e.getBounds().pad(0.035))
    },
    getMarkers(layer) {
      const features = this.schools1516.filter(
        item =>
          item.attributes.NCESSCH.substr(0, 7) ===
          layer.feature.properties.GEOID
      )
      this.markers = features
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
