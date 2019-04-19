<template>
  <div>
    <v-responsive class="light-blue darken-4 white--text">
      <v-container grid-list-lg fluid class="py-5" fill-height>
        <v-layout row wrap justify-center align-center fill-height>
          <v-flex xs12 class="text-xs-center">
            <h1
              :class="
                $vuetify.breakpoint.mdAndUp
                  ? 'headline font-weight-regular'
                  : 'subheading'
              "
            >
              {{ entity.entityDisplayName }}
            </h1>
            <h1
              :class="
                $vuetify.breakpoint.mdAndUp
                  ? 'display-2 font-weight-bold'
                  : 'display-1 font-weight-regular'
              "
            >
              {{ facts[0].componentName }}
            </h1>
            <p :class="$vuetify.breakpoint.mdAndUp ? 'subheading' : 'body-1'">
              {{ facts[0].componentLongDescription }}
            </p>
          </v-flex>
        </v-layout>
      </v-container>
    </v-responsive>
    <v-card tile color="grey lighten-5">
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
            <v-breadcrumbs-item nuxt :to="`/entity/` + entity.entityId">
              {{ entity.entityDisplayName }}
            </v-breadcrumbs-item>
            <v-breadcrumbs-item
              nuxt
              :to="
                `/details/` +
                  facts[0].componentCode.toLowerCase() +
                  '/' +
                  entity.entityId
              "
            >
              {{ facts[0].componentName }}
            </v-breadcrumbs-item>
          </template>
          <template v-if="entity.entityType === 'School'">
            <v-breadcrumbs-item nuxt :to="`/entity/` + entity.parententityId">
              {{ entity.parentEntityName }}
            </v-breadcrumbs-item>
            <v-breadcrumbs-item nuxt :to="`/entity/` + entity.entityId">
              {{ entity.entityDisplayName }}
            </v-breadcrumbs-item>
            <v-breadcrumbs-item
              nuxt
              :to="
                `/details/` +
                  facts[0].componentCode.toLowerCase() +
                  '/' +
                  entity.entityId
              "
            >
              {{ facts[0].componentName }}
            </v-breadcrumbs-item>
          </template>
        </v-breadcrumbs>
      </v-card-text>
    </v-card>

    <section class="pt-5">
      <v-container
        grid-list-md
        class="mb-0 pb-0"
        :fluid="$vuetify.breakpoint.mdAndDown"
        :class="$vuetify.breakpoint.mdAndDown ? 'px-3' : ''"
      >
        <v-layout row wrap justify-center>
          <v-flex xs12 class="text-xs-center">
            <h1 class="display-1 font-weight-bold pb-3">
              {{ facts[0].componentName }} Overview
            </h1>
            <compare-panel
              v-if="
                this.$route.params.component === 'mperf' ||
                  this.$route.params.component === 'eperf' ||
                  this.$route.params.component === 'sperf'
              "
            />
            <svg-chart v-else />
          </v-flex>
        </v-layout>
      </v-container>
    </section>
    <section class="pt-5">
      <v-container
        grid-list-md
        class="mb-0 pb-0"
        :fluid="$vuetify.breakpoint.mdAndDown"
        :class="$vuetify.breakpoint.mdAndDown ? 'px-3' : ''"
      >
        <v-layout row wrap justify-center>
          <v-flex xs12 text-xs-center>
            <h1 class="display-1 font-weight-bold">
              {{ facts[0].componentName }} by Subgroup
            </h1>
            <p class="subheading">
              Small groups are not displayed to protect student privacy. See
              User Guide for more information.
            </p>
            <panel-chart
              v-if="
                this.$route.params.component === 'mperf' ||
                  this.$route.params.component === 'eperf' ||
                  this.$route.params.component === 'sperf'
              "
            />
            <svg-group v-else />
          </v-flex>
        </v-layout>
      </v-container>
    </section>

    <section class="light-blue darken-3">
      <v-container grid-list-md class="mb-0 pb-0">
        <v-layout row wrap justify-center>
          <v-flex xs12 md10 text-xs-center>
            <h1 class="display-1 font-weight-bold white--text my-3">
              {{ facts[0].componentName }} Data
            </h1>
            <!-- <data-table /> -->
          </v-flex>
        </v-layout>
      </v-container>
    </section>
  </div>
</template>

<script>
import allFacts from '@/assets/data/facts.json'
import entities from '@/assets/data/entities.json'
// import { mapGetters } from 'vuex'
import SvgChart from '@/components/charts/SvgChart.vue'
import SvgGroup from '@/components/charts/SvgGroup.vue'
// import DataTable from '@/components/charts/DataTable.vue'
import PanelChart from '@/components/charts/PanelChart.vue'
import ComparePanel from '@/components/charts/ComparePanel.vue'

export default {
  layout: 'light',
  components: {
    SvgChart,
    SvgGroup,
    // DataTable,
    PanelChart,
    ComparePanel
  },
  data() {
    return {
      entities,
      allFacts,
      aggregationCode: []
    }
  },
  computed: {
    entity() {
      const entity = this.entities.filter(
        entity => entity.entityId === this.$route.params.id
      )

      return entity[0]
    },
    facts() {
      const newFacts = this.allFacts.filter(
        fact =>
          fact.componentCode === this.$route.params.component.toUpperCase() &&
          fact.aggregationLevelCategoryCode === 'OVW'
      )
      // eslint-disable-next-line no-console
      console.log('allFacts', this.allFacts)
      // eslint-disable-next-line no-console
      console.log('newFacts', newFacts)
      return newFacts
    }
    // ...mapGetters({ entity: 'getSingleEntity' }),
    // ...mapGetters({ facts: 'getDetailsFacts' })
  },
  // async fetch({ store, params, route }) {
  //   await store.dispatch('callEntity', route.params.id)
  //   await store.dispatch('callDetailsFacts', {
  //     id: route.params.id,
  //     code: route.params.component.toUpperCase()
  //   })
  // },
  // mounted() {
  //   this.buildFacts()
  // },
  methods: {
    buildFacts() {
      this.aggregationCode = this.allFacts
        .filter(
          item =>
            (item.aggregationLevelCategoryCode === 'ETH' ||
              item.aggregationLevelCategoryCode === 'GEN' ||
              item.aggregationLevelCategoryCode === 'OTH' ||
              item.aggregationLevelCategoryCode === 'OVW') &&
            item.entityId === this.$route.params.id
        )
        .map(value => value.aggregationCode)
    }
  }
}
</script>

<style></style>
