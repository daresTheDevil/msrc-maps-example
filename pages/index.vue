<template>
  <div class="grey lighten-5">
    <v-dialog
      v-model="searchDialog"
      fullscreen
      transition="dialog-bottom-transition"
    >
      <v-card flat>
        <v-card-title>
          <v-btn icon @click="searchDialog = false">
            <v-icon>mdi-arrow-left</v-icon>
          </v-btn>
          <v-text-field
            v-if="searchDialog"
            v-model="search"
            solo
            background-color="grey lighten-3"
            flat
            color="primary"
            type="text"
            clearable
            placeholder="Being searching here..."
            hide-details
            autofocus
            :clear-icon-cb="clearSearch"
            @input="onChange"
          />
        </v-card-title>
        <v-list class="mx-0">
          <v-list-tile
            v-for="item in results"
            :key="item.name"
            class="px-0 mx-0"
          >
            <v-layout align-center>
              <v-icon left>
                mdi-star-outline
              </v-icon>

              <h1 class="body-1">
                {{ item.entityDisplayName }}
              </h1>
              <v-spacer />
              <v-btn icon @click="navigate(item.entityId)">
                <v-icon>mdi-chevron-double-right</v-icon>
              </v-btn>
            </v-layout>
          </v-list-tile>
        </v-list>
      </v-card>
    </v-dialog>
    <v-snackbar
      v-model="sheet"
      color="deep-purple"
      :timeout="timeout"
      bottom
      dark
      right
    >
      <h1 class="subheading">{{ selected.length }} / 4 Selected</h1>
      <v-btn dark large block flat round @click="compareEntities">
        Compare
        <v-icon>mdi-chevron-right</v-icon>
      </v-btn>
    </v-snackbar>
    <v-responsive
      class="primary"
      :class="$vuetify.breakpoint.mdAndUp ? 'py-5' : 'pt-3'"
    >
      <v-container grid-list-md fill-height>
        <v-layout row wrap fill-height align-content-space-around>
          <v-flex xs12 class="text-xs-center">
            <p
              class="white--text"
              :class="
                $vuetify.breakpoint.mdAndDown
                  ? 'display-1 font-weight-bold'
                  : 'display-3 font-weight-bold'
              "
            >
              Mississippi Succeeds Report Card
            </p>
            <p
              v-if="$vuetify.breakpoint.mdAndUp"
              class="white--text text-xs-center"
              :class="
                $vuetify.breakpoint.mdAndDown
                  ? 'title font-weight-regular'
                  : 'display-1'
              "
            >
              Search for state, school or district data below.
            </p>
          </v-flex>
        </v-layout>
      </v-container>
    </v-responsive>
    <v-card v-if="$vuetify.breakpoint.mdAndDown" flat color="primary">
      <v-card-text>
        <v-btn large block @click="searchDialog = true">
          <v-icon left> mdi-search-web </v-icon>Search and compare
        </v-btn>
        <v-btn large block to="/map">
          <v-icon left> mdi-map-search </v-icon>Map data
        </v-btn>
        <v-btn large block to="/groupdata">
          <v-icon left> mdi-table-search </v-icon>Compare student groups
        </v-btn>
      </v-card-text>
      <!--
          <v-tabs v-model="tabs" grow icons-and-text height="120" color="transparent">
            <v-tabs-slider color="primary" />
            <v-tab @click="$vuetify.goTo('#card')">
              <h1 class="subheading text-none">
                Search and compare
              </h1>
              <v-icon color="primary">
                mdi-search-web
              </v-icon>
            </v-tab>
            <v-tab @click="$vuetify.goTo('#card')">
              <h1 class="subheading text-none">
                Compare multiple entities
              </h1>
              <v-icon size="64" color="primary">
                mdi-bank-plus
              </v-icon>
            </v-tab>
            <v-tab @click="$vuetify.goTo('#card')">
              <h1 class="subheading text-none">
                Map MSRC Data
              </h1>
              <v-icon size="64" color="primary">
                mdi-map-search
              </v-icon>
            </v-tab>
            <v-tab @click="$vuetify.goTo('#card')">
              <h1 class="subheading text-none">
                Compare student groups
              </h1>
              <v-icon size="64" color="primary">
                mdi-table-search
              </v-icon>
            </v-tab>
          </v-tabs>
        </v-card>
      </v-card-text> -->
    </v-card>
    <v-card
      v-if="$vuetify.breakpoint.mdAndUp"
      id="card"
      max-width="750"
      class="v-card--offset mx-auto elevation-5"
    >
      <v-card-text>
        <single-entity />
      </v-card-text>
    </v-card>
    <!--
        <v-container grid-list-md>
        <v-layout row wrap justify-center>
          <v-flex>
            <v-btn-toggle v-model="searchButton" class="transparent elevation-0 mb-1" mandatory>
              <v-btn flat value="all">
                All
              </v-btn>
              <v-btn flat value="districts">
                Districts
              </v-btn>
              <v-btn flat value="schools">
                Schools
              </v-btn>
            </v-btn-toggle>
            <v-text-field
              v-model="search"
              solo
              background-color="grey lighten-3"
              flat
              color="primary"
              type="text"
              clearable
              placeholder="Being searching here..."
              hide-details
              :clear-icon-cb="clearSearch"
              @input="onChange"
            />
          </v-flex>
        </v-layout>
      </v-container>
    </v-card>
    <v-responsive v-if="$vuetify.breakpoint.mdAndUp" min-height="500">
      <v-container grid-list-lg class="mt-0 pt-0">
        <v-fade-transition v-model="selected" group tag="v-item-group" multiple>
          <v-item v-for="item in computedResults" :key="item.entityId">
            <v-card class="mb-2 elevation-1">
              <v-card-text class="py-1">
                <v-layout row align-center>
                  <v-flex shrink>
                    <v-btn icon @click="toggle(item)">
                      <v-icon
                        v-if="selected.indexOf(item) < 0"
                        color="grey lighten-1"
                        large
                      >
                        mdi-star-outline
                      </v-icon>

                      <v-icon
                        v-else
                        large
                        color="green darken-1"
                      >
                        mdi-star
                      </v-icon>
                    </v-btn>
                  </v-flex>
                  <v-flex shrink>
                    <v-avatar
                      color="amber"
                      class="font-weight-bold white--text headline"
                    >
                      {{ item.entityGrade }}
                    </v-avatar>
                  </v-flex>
                  <v-flex xs4>
                    <v-list-tile-title class="subheading">
                      {{ item.entityDisplayName }}
                    </v-list-tile-title>
                    <v-list-tile-sub-title class="body-1 grey--text text--darken-1">
                      {{ item.parentEntityName }}
                    </v-list-tile-sub-title>
                  </v-flex>
                  <v-spacer />
                  <v-divider vertical inset class="my-3" />

                  <v-flex shrink>
                    <v-icon large>
                      mdi-account
                    </v-icon>
                  </v-flex>
                  <v-flex xs2>
                    <v-list-tile-sub-title class="body-1 grey--text text--darken-1">
                      Superintendent
                    </v-list-tile-sub-title>
                    <v-list-tile-title>{{ item.entityContactName }}</v-list-tile-title>
                  </v-flex>
                  <v-divider vertical inset class="my-3" />
                  <v-flex shrink>
                    <v-icon large>
                      mdi-map-marker
                    </v-icon>
                  </v-flex>
                  <v-flex xs2>
                    <v-list-tile-sub-title class="body-1 grey--text text--darken-1">
                      {{ item.entityAddress }}
                    </v-list-tile-sub-title>
                    <v-list-tile-title>{{ item.entityCity }}, MS {{ item.entityZip }}</v-list-tile-title>
                  </v-flex>
                  <v-flex shrink>
                    <v-btn flat color="primary" @click="navigate(item.entityId)">
                      See details<v-icon>
                        mdi-chevron-right
                      </v-icon>
                    </v-btn>
                  </v-flex>
                </v-layout>
              </v-card-text>
            </v-card>
          </v-item>
        </v-fade-transition>
      </v-container>
    </v-responsive> -->
    <section class="pb-5" :class="$vuetify.breakpoint.mdAndDown ? 'py-5' : ''">
      <v-container grid-list-md fluid>
        <v-layout column>
          <v-flex>
            <v-layout row justify-center>
              <v-flex xs12 md6 class="text-xs-center">
                <h1
                  :class="
                    $vuetify.breakpoint.mdAndDown
                      ? 'headline font-weight-bold'
                      : 'display-1 font-weight-bold'
                  "
                >
                  Statewide Accountability Performance Results
                </h1>
                <p>
                  The Mississippi Statewide Accountability System is a single
                  “A” through “F” school and district accountability system.
                  Grades are based on student achievement, student growth,
                  student participation in testing, and other academic measures.
                </p>
              </v-flex>
            </v-layout>
          </v-flex>
          <v-flex v-if="$vuetify.breakpoint.mdAndDown">
            <v-list three-line class="transparent">
              <v-list-tile
                v-for="item in links"
                :key="item.text"
                nuxt
                :to="item.to"
              >
                <v-list-tile-avatar :color="item.color" class="mt-1" size="46">
                  <h1
                    v-if="!item.icon"
                    class="display-1 font-weight-bold white--text"
                  >
                    C
                  </h1>
                  <v-icon v-if="item.icon" dark>
                    {{ item.icon }}
                  </v-icon>
                </v-list-tile-avatar>
                <v-list-tile-content>
                  <v-list-tile-title v-text="item.title" />
                  <v-list-tile-sub-title class="body-1" v-text="item.text" />
                </v-list-tile-content>
              </v-list-tile>
            </v-list>
          </v-flex>

          <v-flex v-if="$vuetify.breakpoint.mdAndUp">
            <v-layout row wrap>
              <v-flex xs1 />
              <v-flex v-for="item in links" :key="item.title" xs2>
                <v-card color="transparent" flat class="text-xs-center">
                  <v-card-text class="text-xs-center px-0">
                    <v-avatar size="78" :color="item.color">
                      <h1
                        v-if="!item.icon"
                        class="display-2 font-weight-bold white--text"
                      >
                        C
                      </h1>
                      <v-icon v-if="item.icon" size="52" dark>
                        {{ item.icon }}
                      </v-icon>
                    </v-avatar>
                  </v-card-text>
                  <v-card-text class="py-1">
                    <h1 class="title" :class="item.textcolor">
                      {{ item.title }}
                    </h1>
                  </v-card-text>
                  <v-card-text class="py-1 px-0">
                    <p>
                      {{ item.text }}
                    </p>
                  </v-card-text>
                  <v-card-actions class="justify-space-around py-1">
                    <v-btn round dark :color="item.color">
                      {{ item.buttonText }}
                      <v-icon v-if="item.buttonIcon" right>
                        {{ item.buttonIcon }}
                      </v-icon>
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-flex>
              <v-flex xs1 />
            </v-layout>
          </v-flex>
        </v-layout>
      </v-container>
    </section>
    <section class="primary white--text py-5">
      <v-container grid-list-lg>
        <v-layout column justify-center>
          <v-flex>
            <v-layout row justify-center>
              <v-flex xs12 md6 class="text-xs-center">
                <h1
                  :class="
                    $vuetify.breakpoint.mdAndDown
                      ? 'headline font-weight-bold'
                      : 'display-1 font-weight-bold'
                  "
                >
                  The Data
                </h1>
                <p>
                  Every school and district has an overall accountability grade
                  and score. The data include assessment, teacher, and
                  environment data that gives an overview of school performance
                  with the ability to link to more detailed data.
                </p>
              </v-flex>
            </v-layout>
          </v-flex>
          <v-flex>
            <v-layout align-start>
              <v-item-group v-model="window" class="mr-4" mandatory tag="div">
                <v-item>
                  <div slot-scope="{ active, toggle }">
                    <v-card
                      :input-value="active"
                      class="my-2"
                      :color="active ? 'white' : 'primary lighten-2'"
                      :class="active ? 'elevation-10' : 'elevation-0'"
                      width="400"
                      @click="toggle"
                    >
                      <v-card-title class="title pb-1">
                        The Hero
                      </v-card-title>
                      <v-divider />
                      <v-card-text class="pt-1">
                        The hero shows basic information like address, contact
                        information and accountability grade.
                      </v-card-text>
                    </v-card>
                  </div>
                </v-item>
                <v-item>
                  <div slot-scope="{ active, toggle }">
                    <v-card
                      :input-value="active"
                      class="my-2"
                      :color="active ? 'white' : 'primary lighten-2'"
                      :class="active ? 'elevation-10' : 'elevation-0'"
                      width="400"
                      @click="toggle"
                    >
                      <v-card-title class="title pb-1">
                        Detailed data
                      </v-card-title>
                      <v-divider />
                      <v-card-text class="pt-1">
                        Click a tile to access detailed information including
                        comparisons and breakdowns by group.
                      </v-card-text>
                    </v-card>
                  </div>
                </v-item>
                <v-item>
                  <div slot-scope="{ active, toggle }">
                    <v-card
                      :input-value="active"
                      class="my-2"
                      :color="active ? 'white' : 'primary lighten-2'"
                      :class="active ? 'elevation-10' : 'elevation-0'"
                      width="400"
                      @click="toggle"
                    >
                      <v-card-title class="title pb-1">
                        Group comparison
                      </v-card-title>
                      <v-divider />
                      <v-card-text class="pt-1">
                        Select varied datasets to see academic performance by
                        various student groups.
                      </v-card-text>
                    </v-card>
                  </div>
                </v-item>
              </v-item-group>

              <v-flex xs8>
                <v-window v-model="window" vertical>
                  <v-img
                    src="/images/msrc-browser-background.svg"
                    contain
                    class="pt-3"
                  >
                    <v-window-item>
                      <v-container fill-height fluid class="px-0">
                        <v-layout fill-height align-center>
                          <v-flex class="px-0">
                            <img src="/images/msrc-header.svg" />
                          </v-flex>
                        </v-layout>
                      </v-container>
                    </v-window-item>
                    <v-window-item>
                      <v-container fill-height fluid class="px-0">
                        <v-layout fill-height align-center>
                          <v-flex class="px-0">
                            <img src="/images/msrc-section.svg" width="100%" />
                          </v-flex>
                        </v-layout>
                      </v-container>
                    </v-window-item>
                    <v-window-item>
                      <v-container fill-height fluid class="px-0">
                        <v-layout fill-height align-center>
                          <v-flex class="px-0">
                            <img src="/images/msrc-crosstab.svg" />
                          </v-flex>
                        </v-layout>
                      </v-container>
                    </v-window-item>
                  </v-img>
                </v-window>
              </v-flex>
            </v-layout>
          </v-flex>
        </v-layout>
        <br />
      </v-container>
    </section>
  </div>
</template>

<script>
import SingleEntity from '@/components/search/SingleEntity.vue'
import entities from '@/assets/data/entities.json'

export default {
  components: { SingleEntity },
  data() {
    return {
      timeout: 0,
      entities,
      search: '',
      sheet: false,
      searchButton: 'all',
      results: [],
      selected: [],
      searchDialog: false,
      length: 3,
      window: 0,
      interval: null,
      links: [
        {
          title: 'Mississippi Average',
          text: 'Learn more about the state results and subgroup breakdowns.',
          to: '/entity/0000-000',
          color: 'teal',
          textcolor: 'teal--text',
          buttonText: 'See State Data'
        },
        {
          title: 'All Data',
          text: 'Download the full dataset in spreadsheet format.',
          to: '/',
          icon: 'mdi-cloud-download',
          color: 'red darken-4',
          textcolor: 'red--text text-darken-4',
          buttonText: 'Download all data'
        },
        {
          title: 'Student Group Data',
          text: 'Review academic performance by various student groups.',
          to: '/groupdata',
          icon: 'mdi-filter',
          color: 'primary darken-2',
          textcolor: 'primary--text text-darken-2',
          buttonText: 'Compare student groups'
        },
        {
          title: 'Video Overview',
          text:
            'A quick video with highlights of the Mississippi Succeeds Report Card.',
          to: '/',
          icon: 'mdi-presentation-play',
          color: 'amber darken-2',
          textcolor: 'amber--text text-darken-2',
          buttonText: 'Watch overview',
          buttonIcon: 'mdi-open-in-new'
        },
        {
          title: 'User Guide',
          text: 'Get an in-depth understanding of report card details.',
          to: '/',
          icon: 'mdi-information-outline',
          color: 'deep-orange darken-2',
          textcolor: 'deep-orange--text text-darken-2',
          buttonText: 'Read guide'
        }
      ]
    }
  },
  computed: {
    computedResults() {
      if (this.searchButton === 'districts')
        return this.results.filter(item => item.entityType === 'District')
      else if (this.searchButton === 'schools')
        return this.results.filter(item => item.entityType === 'School')
      return this.results
    }
  },
  // async fetch({ store, params }) {
  //   await store.dispatch('callAllEntities')
  // },
  mounted() {
    this.results = this.entities
  },
  methods: {
    compareEntities() {},
    clearSearch() {
      this.search = ''
      this.results = []
    },
    onChange() {
      this.filterResults()
    },
    filterResults() {
      this.results = this.entities.filter(
        item =>
          item.entityDisplayName
            .toLowerCase()
            .indexOf(this.search.toLowerCase()) > -1 ||
          item.entityCity.toLowerCase().indexOf(this.search.toLowerCase()) >
            -1 ||
          item.entityId.toLowerCase().indexOf(this.search.toLowerCase()) > -1
      )
    },
    navigate(id) {
      this.$router.push({
        name: 'entity-id',
        params: { id: id }
      })
    }
  }
}
</script>

<style>
.v-card--offset {
  top: -40px;
  position: relative;
}
</style>
