<template>
  <div id="capture" ref="capture" v-scroll="onScroll">
    <v-toolbar
      v-if="$vuetify.breakpoint.smAndUp"
      app
      dark
      dense
      color="primary"
      inverted-scroll
      class="pb-0"
      style="z-index: 999;"
    >
      <h1 class="caption">
        {{ scroll }}
      </h1>
      <v-tabs
        v-model="scrollTab"
        fixed-tabs
        grow
        color="transparent"
        active-class="primary darken-3"
        hide-slider
      >
        <v-tab @click="$vuetify.goTo('#proficiency', selector)">
          <h1 class="subheading text-capitalize">
            Proficiency
          </h1>
        </v-tab>
        <v-tab @click="$vuetify.goTo('#growth', selector)">
          <h1 class="subheading text-capitalize">
            Growth
          </h1>
        </v-tab>
        <v-tab>
          <h1 class="subheading text-capitalize">
            Progress & Readiness
          </h1>
        </v-tab>
        <v-tab>
          <h1 class="subheading text-capitalize">
            Assessment Results
          </h1>
        </v-tab>
        <v-tab>
          <h1 class="subheading text-capitalize">
            School Climate
          </h1>
        </v-tab>
      </v-tabs>
    </v-toolbar>
    <v-responsive class="light-blue darken-4 white--text">
      <v-container
        grid-list-xl
        :class="$vuetify.breakpoint.mdAndUp ? 'py-5' : ''"
        fill-height
      >
        <v-layout row wrap align-center>
          <v-flex>
            <v-layout row wrap fill-height align-center>
              <v-flex xs12>
                <v-layout row align-center>
                  <v-flex v-if="$vuetify.breakpoint.mdAndUp" shrink>
                    <v-avatar color="white" size="64">
                      <h1
                        class="display-2 font-weight-black primary--text"
                        v-text="entity.entityGrade"
                      />
                    </v-avatar>
                  </v-flex>
                  <v-flex>
                    <h1
                      :class="
                        $vuetify.breakpoint.mdAndUp
                          ? 'display-2 font-weight-bold'
                          : 'headline font-weight-regular'
                      "
                    >
                      {{ entity.entityDisplayName }}
                    </h1>
                    <h1
                      v-if="entity.entityType === 'School'"
                      :class="
                        $vuetify.breakpoint.mdAndUp
                          ? 'title font-weight-regular'
                          : 'subheading'
                      "
                    >
                      {{ entity.parentEntityName }}
                    </h1>
                  </v-flex>
                </v-layout>
              </v-flex>
              <v-flex xs12 md3>
                <v-img :aspect-ratio="$vuetify.breakpoint.mdAndUp ? 1 : 2">
                  <l-map
                    ref="map"
                    :options="mapOptions"
                    :zoom.sync="zoom"
                    :center.sync="center"
                    class="mapContainer"
                  >
                    <l-tile-layer :url="standardTiles" />
                    <l-marker :lat-lng="center" />
                  </l-map>
                </v-img>
              </v-flex>
              <v-flex shrink>
                <v-list class="transparent" dark>
                  <v-list-tile v-if="$vuetify.breakpoint.mdAndDown">
                    <v-list-tile-avatar color="white">
                      <h1
                        class="display-1 font-weight-black primary--text"
                        v-text="entity.entityGrade"
                      />
                    </v-list-tile-avatar>
                    <v-list-tile-content>
                      <v-list-tile-title
                        >Accountability grade</v-list-tile-title
                      >
                    </v-list-tile-content>
                  </v-list-tile>
                  <v-divider v-if="$vuetify.breakpoint.mdAndDown" inset />
                  <v-list-tile>
                    <v-list-tile-avatar color="white">
                      <v-icon medium color="green darken-1">
                        mdi-trending-up
                      </v-icon>
                    </v-list-tile-avatar>
                    <v-list-tile-content>
                      <v-list-tile-title
                        >Improved over last year</v-list-tile-title
                      >
                    </v-list-tile-content>
                  </v-list-tile>
                  <v-divider inset />

                  <v-list-tile>
                    <v-list-tile-avatar color="white">
                      <v-icon color="primary">
                        mdi-map-marker
                      </v-icon>
                    </v-list-tile-avatar>
                    <v-list-tile-content>
                      <v-list-tile-sub-title>{{
                        entity.entityAddress
                      }}</v-list-tile-sub-title>
                      <v-list-tile-sub-title
                        >{{ entity.entityCity }}, MS
                        {{ entity.entityZip }}</v-list-tile-sub-title
                      >
                    </v-list-tile-content>
                  </v-list-tile>
                  <v-divider inset />
                  <v-list-tile>
                    <v-list-tile-avatar color="white">
                      <v-icon color="primary">
                        mdi-account
                      </v-icon>
                    </v-list-tile-avatar>
                    <v-list-tile-content>
                      <v-list-tile-title>{{
                        entity.entityContactName
                      }}</v-list-tile-title>
                      <v-list-tile-sub-title
                        v-if="entity.entityType !== 'State'"
                      >
                        {{ entity.entityContactEmail }}
                      </v-list-tile-sub-title>
                    </v-list-tile-content>
                  </v-list-tile>
                  <v-divider inset />
                  <v-list-tile @click="print">
                    <v-list-tile-avatar color="white">
                      <v-icon color="primary">
                        mdi-file-pdf
                      </v-icon>
                    </v-list-tile-avatar>
                    <v-list-tile-content>
                      <v-list-tile-title>Report Card Summary</v-list-tile-title>
                    </v-list-tile-content>
                  </v-list-tile>
                  <template v-if="entity.entityType === 'State'">
                    <v-divider inset />
                    <v-list-tile nuxt to="/entity/crosstab">
                      <v-list-tile-avatar>
                        <v-icon medium>
                          mdi-filter
                        </v-icon>
                      </v-list-tile-avatar>
                      <v-list-tile-content>
                        <v-list-tile-sub-title
                          >Data Comparison Tool</v-list-tile-sub-title
                        >
                      </v-list-tile-content>
                    </v-list-tile>
                  </template>
                </v-list>
              </v-flex>
            </v-layout>
          </v-flex>
        </v-layout>
      </v-container>
    </v-responsive>
    <v-card color="grey lighten-5">
      <v-card-text class="pa-0">
        <v-breadcrumbs large divider=">">
          <v-breadcrumbs-item nuxt to="/">
            <v-icon medium>
              mdi-home
            </v-icon>
          </v-breadcrumbs-item>
          <v-breadcrumbs-item nuxt to="/entity/0000-000">
            Mississippi
          </v-breadcrumbs-item>
          <template v-if="entity.entityType === 'District'">
            <v-breadcrumbs-item v-text="entity.entityDisplayName" />
          </template>
          <template v-if="entity.entityType === 'School'">
            <v-breadcrumbs-item nuxt :to="`/entity/` + entity.parentEntityId">
              {{ entity.parentEntityName }}
            </v-breadcrumbs-item>
            <v-breadcrumbs-item v-text="entity.entityDisplayName" />
          </template>
        </v-breadcrumbs>
      </v-card-text>
    </v-card>

    <!--
    <v-responsive id="proficiency" class="teal darken-1 white--text">
      <v-container>
        <h1 class="display-3 font-weight-bold">
          Proficiency
        </h1>
        <h1 class="headline font-weight-regular pt-0">
          How well did students score on state assessments?
        </h1>
      </v-container>
    </v-responsive>

    <proficiency-section />

    <v-responsive id="growth" class="cyan darken-1 white--text">
      <v-container>
        <h1 class="display-3 font-weight-bold">
          Growth
        </h1>
        <h1 class="headline font-weight-regular pt-0">
          Are assessment scores improving?
        </h1>
      </v-container>
    </v-responsive>
    <growth-section />

    <proficiency-section /> -->

    <accountability-grade />

    <assessment-results v-if="show" />

    <other-measures v-if="show" />

    <section v-if="entity.entityType == 'District'" class="light-blue darken-4">
      <v-container class="py-5">
        <v-layout row wrap justify-center>
          <v-flex shrink>
            <h1
              :class="$vuetify.breakpoint.mdAndUp ? 'display-2' : 'display-1'"
              class="text-xs-center white--text font-weight-bold"
            >
              Schools in this district
            </h1>
            <br />
            <v-list class="transparent" dark>
              <v-list-tile
                v-for="item in districtSchools"
                :key="item.entityId"
                nuxt
                :to="`/entity/` + item.entityId"
              >
                <v-list-tile-content>
                  <v-list-tile-title
                    :class="
                      $vuetify.breakpoint.mdAndUp
                        ? 'title font-weight-light'
                        : 'subheading'
                    "
                    v-text="item.entityDisplayName"
                  />
                  <v-list-tile-sub-title
                    >{{ item.entityCity }}, MS</v-list-tile-sub-title
                  >
                </v-list-tile-content>
              </v-list-tile>
            </v-list>
          </v-flex>
        </v-layout>
      </v-container>
    </section>
  </div>
</template>

<script>
// import { mapGetters } from 'vuex'
import entities from '@/assets/data/entities.json'
import AccountabilityGrade from '@/components/sections/AccountabilityGrade.vue'
import AssessmentResults from '@/components/sections/AssessmentResults.vue'
import OtherMeasures from '@/components/sections/OtherMeasures.vue'
// import ProficiencySection from '@/components/sections/ProficiencySection.vue'
// import GrowthSection from '@/components/sections/GrowthSection.vue'

export default {
  layout: 'light',
  components: {
    AccountabilityGrade,
    AssessmentResults,
    OtherMeasures
    // ProficiencySection,
    // GrowthSection
  },
  data() {
    return {
      entities,
      drawer: true,
      center: [32.245329, -89.862671],
      zoom: 7.5,
      standardTiles: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      lightTiles:
        'https://cartodb-basemaps-{s}.global.ssl.fastly.net/light_all/{z}/{x}/{y}.png',
      shows: false,
      value: [423, 446, 675, 510, 590, 610, 760],
      show: false,
      map: null,
      scroll: '',
      mapOptions: {
        zoomSnap: 0.5,
        zoomControl: false,
        zoomDelta: 0.5,
        dragging: false
      },
      scrollTab: 1,
      gradient: ['purple', 'violet'],
      fill: true,
      items: [
        {
          text: 'Dashboard',
          disabled: false,
          href: 'breadcrumbs_dashboard'
        },
        {
          text: 'Link 1',
          disabled: false,
          href: 'breadcrumbs_link_1'
        },
        {
          text: 'Link 2',
          disabled: true,
          href: 'breadcrumbs_link_2'
        }
      ]
    }
  },
  computed: {
    entity() {
      const entity = this.entities.filter(
        entity => entity.entityId === this.$route.params.id
      )
      return entity[0]
    },
    districtSchools() {
      return this.entities.filter(
        entity => entity.parentEntityId === this.$route.params.id
      )
    }
    // ...mapGetters({ entity: 'getSingleEntity' }),
    // ...mapGetters({ districtSchools: 'getDistrictSchools' })
  },
  async fetch({ store, params, route }) {
    // await store.dispatch('callEntity', route.params.id)
    // await store.dispatch('callDistrictSchools', route.params.id)
    // await store.dispatch('callLandingFacts', route.params.id)
  },
  mounted() {
    this.$nextTick(() => {
      this.map = this.$refs.map.mapObject
      // this.map.dragging.disable()
      this.map.setMinZoom(7).setMaxZoom(17)
    })
  },
  methods: {
    onScroll() {
      if (typeof window === 'undefined') return
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      const scroll =
        window.pageYOffset || document.documentElement.offsetTop || 0
      if (scroll < 300) {
        this.scrollTab = 0
      } else if (scroll >= 300) {
        this.scrollTab = 0
      }
    }
  }
}
</script>

<style></style>
