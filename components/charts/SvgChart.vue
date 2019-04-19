<template>
  <div>
    <v-layout row justify-center class="mb-3">
      <v-chip color="amber">
        State
      </v-chip>
      <v-chip color="teal accent-3">
        District
      </v-chip>
      <v-chip color="light-blue accent-2">
        School
      </v-chip>
      <v-chip color="grey darken-1 white--text">
        2025 Goal - {{ facts[0].componentLongTermGoal }}%
      </v-chip>
    </v-layout>
    <v-tooltip top dark color="grey darken-4">
      <div v-for="(item, i) in facts" :key="item._id" slot="activator">
        <v-progress-linear
          :value="item.componentValue"
          :buffer-value="bufferValue(item)"
          :color="color[i]"
          height="45"
          :background-color="
            item.componentLongTermGoal ? 'grey' : 'grey lighten-2'
          "
          class="grey lighten-2 text-xs-left my-1"
          buffer
        >
          <v-layout justify-space-between>
            <h1 class="title font-weight-regular pa-3" v-text="entityType[i]" />
            <h1 class="title font-weight-regular pa-3">
              {{ item.componentValue }}%
            </h1>
          </v-layout>
        </v-progress-linear>
      </div>
      <v-card color="transparent" dark flat>
        <v-card-text class="title font-weight-regular pb-0">
          Overview
        </v-card-text>
        <v-card-text class="pt-0">
          <v-list class="transparent">
            <v-list-tile v-for="(item, index) in facts" :key="index">
              <v-list-tile-avatar>
                <v-chip :color="color[index]" class="black--text">
                  {{ item.componentValue }}%
                </v-chip>
              </v-list-tile-avatar>
              <v-list-tile-title>{{
                item.entityDisplayName
              }}</v-list-tile-title>
            </v-list-tile>
            <v-list-tile>
              <v-list-tile-avatar>
                <v-chip color="grey darken-2">
                  {{ facts[0].componentLongTermGoal }}%
                </v-chip>
              </v-list-tile-avatar>
              <v-list-tile-title>2025 Goal</v-list-tile-title>
            </v-list-tile>
          </v-list>
        </v-card-text>
      </v-card>
    </v-tooltip>
  </div>
</template>

<script>
// import { mapGetters } from 'vuex'
import allFacts from '@/assets/data/facts.json'
import entities from '@/assets/data/entities.json'

export default {
  data() {
    return {
      color: ['amber', 'teal accent-3', 'light-blue accent-2'],
      entityType: ['State', 'District', 'School'],
      allFacts,
      entities
    }
  },
  computed: {
    // ...mapGetters({ entity: 'getSingleEntity' }),
    // ...mapGetters({ allFacts: 'getDetailsFacts' }),
    entity() {
      return this.entities.filter(
        entity => entity.entityId === this.$route.params.id
      )
    },
    facts() {
      return this.allFacts
        .filter(
          item =>
            item.componentCode === this.$route.params.component.toUpperCase() &&
            item.aggregationLevelCategoryCode === 'OVW' &&
            (item.entityId === this.$route.params.id ||
              item.entityId === '0000-000' ||
              item.entityId === item.parentEntityId)
        )
        .reverse()
      // .reverse()
    }
  },
  methods: {
    bufferValue(item) {
      if (item.componentLongTermGoal) return item.componentLongTermGoal
    }
  }
}
</script>

<style></style>
